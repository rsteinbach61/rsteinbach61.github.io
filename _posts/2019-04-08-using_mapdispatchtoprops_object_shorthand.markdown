---
layout: post
title:      "Using mapDispatchToProps Object Shorthand"
date:       2019-04-08 16:22:24 -0400
permalink:  using_mapdispatchtoprops_object_shorthand
---



I discovered a nice shorthand available when dispatching in components. In my example I’m dispatching a few actions in my RoomsContainer that retrieve, add, and remove a Room respectively. 

Instead of using these two lines:
```
const mapDispatchToProps = {getRoom, addRoom, removeRoom}

export default connect(mapStateToProps, mapDispatchToProps) (RoomsContainer)

```

As long as my action creators return objects, I can simplify to:
```
export default connect(mapStateToProps, {getRoom, addRoom, removeRoom} ) (RoomsContainer)

```

Let’s look at the getRoom function:
```
export function getRoom(id) {
  return function(dispatch) {
    return getRooms(id).then(rooms =>{
        dispatch(getRoomsSuccess(payload))
    })
  }
}

```

It calls getRooms, a fetch that retrieves Rooms from my DB, and then dispatches an action named getRoomsSuccess and passes the Rooms retrieved from the DB as payload. 
```
export function getRoomsSuccess(payload){
  return {type: "GET_ROOMS_SUCCESS", payload}
}

```

getRoomsSuccess returns an object containing a type and a payload to getRoom which ultimately passes it through to mapDispatchToProps. Since getRoom returns an object with everything my reducer needs, Connect lets me simply add it in line with mapStateToProps.

From the [React Redux Docs](https://react-redux.js.org/)

“We recommend always using the “object shorthand” form of mapDispatchToProps, unless you have a specific reason to customize the dispatching behavior.”


[More from on object shorthand from the React Redux Docs](https://react-redux.js.org/using-react-redux/connect-mapdispatch#defining-mapdispatchtoprops-as-an-object
)

