---
layout: post
title:  "[ReactNative] To Do App 코드 리뷰하기 "
date:   2019-06-07 19:16:00
author: 한만섭
categories: reactnative
tags: reactnative code review
---

* TOC
{:toc}


> ### App.js에서 하위 컴포넌트로 함수 넘기기 
App.js에서 만든 함수를 하위 컴포넌트의 prop으로 넘겨주어야 하는 상황이 있었다.  
To Do 객체의 isCompleted 를 true 혹은 false로 바꾸어 주어야 하는 상황이었다.  

우선 `_completeToDo`함수의 모습을 리뷰해보겠다.  
_completeToDo => this.state의 toDos객체중에 특정 id를 갖고있는 toDo객체 의 iscompleted를 true로 바꾸고 싶음.
다른 state들은 건드리지 않고 toDos만 수정 , 그 중에서도 특정 id 만 갖고 있는 toDo를 수정 그 중에서도 
iscompleted속성만 true로 변경  
원하는 속성만 변경하기 위해 prevState를 이용함. prevState는 따로 제대로 다뤄야 될 것 같음.  


```
_completeToDo = (id) =>{ 
    this.setState ( prevState =>{   //현재 컴포넌트의 state를 수정하겠다. 인자로 prevState를 사용.
      const newState = { // 새로운 state를 return 해야하기 때문에 새로운 state생성 
        ...prevState, // 기존의 state는 유지한채로 
        toDos:{ // toDos객체를 만나면 덮어씌어주겠음. 
          ...prevState.toDos, // toDos객체중에서도 특정 조건 아니면 건너뜀.
          [id] : { // 인자로 받은 id 를 가진 toDo를 덮어씌우겠음. 
            ...prevState.toDos[id], // toDo중에서도 내가 원하는 속성만 덮어씌울거임.
            isCompleted : true // isCompleted만 true로 셋팅하겠음. 
          }
        }
      }
      return {...newState} // 새로 생성한 newState를 this의 state로 return 하기 
    });
  };
```  

　  
   
   
# >>> 객체를 통해서 하위컴포넌트를 생성하기 
toDos라는 object를 통해서 Todo라는 하위 컴포넌트를 여러개 만들어야 했다. 그러기 위해서는 반복적인 코딩이 필요하다.  
하지만 object를 통해서 하위 컴포넌트들을 생성할 때는 효율적으로 할 수 있다.  

```
Object.values(toDos).map(toDo => <Todo></Todo>)} // toDos라는 객체를 toDo로 맵핑하고 Todo컴포넌트를 쓰겠다.
```
```
for(toDo in toDos) //이 느낌과 동일함
```

```
  <ScrollView contentContainerStyle={styles.toDos}>
             {Object.values(toDos).map(toDo => 
             <Todo
              key={toDo.id}
              deleteToDo={this._deleteTodo}
              completeToDo={this._completeToDo}
              uncompleteToDo={this._uncompleteToDo}  // 함수도 prop로 넘기는 것이 가능함. 
              {...toDo} // prop을 pass 한다고 하는데 정확히 무슨 뜻인지 모르겠음. 
                ></Todo>)}
           </ScrollView>
```



> ### 하위 컴포넌트에서 function받기 
아래 코드 처럼 상위 컴포넌트는 하위 컴포넌트에게 functnion도 전달 가능함.
```
    static propTypes = {
        text : propTypes.string.isRequired,
        isCompleted : propTypes.bool.isRequired,   
        deleteToDo : propTypes.func.isRequired,
        completeToDo : propTypes.func.isRequired,
        uncompleteToDo : propTypes.func.isRequired,
        id : propTypes.string.isRequired
    }
```


> ### render()에서 prop혹은 state사용하기 
```
 const {isEditing,toDoValue} = this.state;
const {text,deleteToDo,id ,isCompleted} = this.props;
```

> ### TouchableOpacity 에서 click event 
```
<TouchableOpacity onPressOut={this._toggleComplete}>
      <View style={[styles.circle, isCompleted ? styles.completeCircle : styles.uncompletedCircle]}></View>
</TouchableOpacity>
```


> ### 메서드 에서 state및 prop사용하기 
```
  _toggleComplete = () => {
        //isCompleted를 App.js에서 직접 주지 않지만 toDos의 object안에 있는 속성이라 알아서 props로 해주는것 같음. 
      const {isCompleted,completeToDo,uncompleteToDo , id} = this.props; 
      if(isCompleted){ //완료한 항목 클릭시 -> 완료안한 상태로 바꿈
        uncompleteToDo(id);
      }else { // 완료 안한 항목 클릭시 -> 완료 상태로 바꿈 
        completeToDo(id); 
      }
```
