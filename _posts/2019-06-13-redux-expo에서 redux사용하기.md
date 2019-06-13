---
layout: post
title:  "[redux] expo프로젝트에서 redux 사용하기"
date:   2019-06-13 16:37:00
author: 한만섭
categories: redux
tags: expo redux reactnative
---

> ## 시작하기전에... 
redux를 사용하려고 이것저것 시도해봤는데 redux에 대한 개념이 제대로 잡히지도 않은채로 시도하다보니 삽질을 정말 많이했다.  
결론적으로 깨달은 것은 `component`를 `export default`해서 `App.js`에서 사용하는 것이 아니라, `connect`를 해서 사용해야한다는 것이다.  
　  

***

　  
   
> ### App.js 
우선 각 파일 별로 코드를 먼저 살펴보겠다.  
```
import React  from 'react'; // react import
import Timer from "./components/Timer/presenter"; //Timer component를 사용하기위해 import 
import reducer from "./reducer"; // 직접 만든 reducer를 사용하기 위함. 
import { createStore} from 'redux'; // reducer를 store로 만들기 위해 사용 
import { Provider} from "react-redux"; // store를 component(Timer)에 연결하기 위해 사용 

const store = createStore(reducer); // createStore를 이용해 reducer를 store로 만들어서 const 변수에 넣음. 
console.log(store.getState()); // getState를 하기 위해서는 reducer의 default일 때 state를 주어야함.  // store의 state를 확인하기위한 코드

export default class App extends React.Component {

  render(){
    return (
      <Provider store={store}> // Timer component 를 Provider로 감싼다. 
        <Timer />
      </Provider>
    );  
  }
}
```  
　  

***

　  
> ### presenter.js
```
import React , {Component} from "react";
import {View,Text,StyleSheet,StatusBar} from "react-native";
// import * as Font from "expo-font";
import Button from "../Button/Button";
import {connect} from 'react-redux'; // component에 store를 연결하기위해 필요함. 

// presenter.js 에서 직접 연결할 것이기 때문에 필요함. 
const mapStateToProps = state =>
 ({ 
     isPlaying: state.isPlaying,
    elapsedTime : state.elapsedTime,
    timerDuration : state.timerDuration
})

 class Timer extends Component {

    // async componentDidMount(){
    //     await Font.loadAsync({'BM-font': require('../../assets/fonts/BMHANNAPro.ttf')});
    // }
    
    render() {
        // console.log(this.props);
        const {isPlaying , elapsedTime , timerDuration} = this.props;
        console.log(isPlaying , elapsedTime, timerDuration)
        return(
            <View style = { styles.container}>
                <StatusBar barStyle={"light-content"}></StatusBar>
                <View style = { styles.upper}>
                    <Text style = { styles.time}>25:00</Text>
                </View>
                <View style = { styles.lower}>
                    <Button iconName={"play-circle"} onPress={()=> alert("it works")}></Button>
                    <Button iconName={"stop-circle"} onPress={()=> alert("it works")}></Button>
                </View>
            </View>
        )
    }
}

const styles = StyleSheet.create({
    container : {
        flex : 1,
        
        backgroundColor : "#CE0B24"
    },
    upper : {
        flex : 1,
        justifyContent : "center",
        alignItems : "center",
        backgroundColor : "#CE0B24"
    },
    time : {
        color : "white",
        fontSize : 120,
        // fontFamily : "BM-font",
        fontWeight : "normal"
    },
    lower : {
        flex: 1,
        fontSize : 50,
        color : "white",
        alignItems : "center",
        justifyContent : "center",
        backgroundColor : "#CE0B24"
    }
})
export default connect(mapStateToProps)(Timer); // export default Timer; 대신에 connect해서 export해야하는 것이 문제였음. 
```  

* 처음에는 아래와 같이 Timer 클래스를 export했는데 이렇게 할 경우 connect가 되지 않은 상태로 App.js에서 import를 해서 사용하였다.
```
export default Timer;
```

* 해결방법 
- Timer component를 store에 연결한 채로 export 해야했기 때문에 아래 코드로 export를 변경하여야 했다.  
 ```
 export default connect(mapStateToProps)(Timer);
 ```
 



