---
layout: post
title:  "Adding Like Functionality to JetLog + Custom Markers [React + Redux]"
date:   2017-09-28 17:25:36 +0000
---


My app JetLog — a Google maps integrated memory map for users to plot their travels, was finished, well at least the foundation of it was. Users could create and sign-in to accounts and create and view their travels “Logs” in both a stream and as a pin on a map.
A Log in Hong KongCustom Marker
A firm believer that the little touches can make all the difference, I wanted to take ownership of the Google Maps Marker. In my case I was utilizing react-google-maps as a React component wrapper to persist the Google Maps API. The Marker component allows for a prop of Icon which one can simply pass in an image as a named import into.

```
/* eslint-disable no-undef */
import React from 'react';

import { compose, withProps, lifecycle, withStateHandlers } from 'recompose';
import { withGoogleMap, GoogleMap, InfoWindow, Marker } from 'react-google-maps';
import withScriptjs from 'react-google-maps/lib/async/withScriptjs';

import LogCard from '../views/Logs/LogCard'

import logMark from '../assets/marker.svg'

...

const Map = compose(
... 

  {props.markers.map(marker => (
    <Marker
      icon={logMark}
      key={marker.id}
      position={{ lat: parseFloat(marker.latitude), lng: parseFloat(marker.longitude) }}
      onClick={props.onToggleOpen}
    >
    ...
    </Marker>
  ))}
  </GoogleMap>
);

export default Map

```

Dropping in a marker template in Sketch, adding the JetLog branding, and resizing here and there until I was content, I had my marker.
Marker w/ JetLog Paper Plane LogoAdding Likes +1
As we all know, no app is complete until people can like things. I kid, but adding the ability for user’s to like a log would allow me to explore Patch Requests and provide additional nuance to my application while garnering a better grasp of Redux reducers.
To start I would simply establish a basic non-Redux Like component:

```
import React from 'react'

import RaisedButton from 'material-ui/RaisedButton';

const API_URL = process.env.REACT_APP_API_URL;

class Like extends React.Component {
  constructor(){
    super();

    this.state = { like: 0 }
    this.increment = this.increment.bind(this)
  }
  increment(){
    this.setState({ like: this.state.like + 1})
  }

  render(){
    console.log('render')
    return (
      <div>
        {/* <button onClick={this.handleApiCall}>Api Call</button> */}
        <button onClick={this.increment}>{this.state.like}</button>
      </div>
    )
  }
}

export default Like;
```



2. Then I would get it working in Redux:

```
# Actions.js - src/redux/modules/Likes/actions.js

export const increment = () => {
  return {
    type: 'INCREMENT_LIKE',
  }
}

# Reducer.js - src/redux/modules/Likes/reducer.js

const likesReducer = (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT_LIKE':
      return state + 1;
    default:
      return state
  }
}

export default likesReducer;

# Likes.js - src/views/Logs/Likes.js

import React from 'react'
import { connect } from 'react-redux';
import { increment } from '../../redux/modules/Likes/actions'

class Likes extends React.Component {

  render(){
    return (
      <div>
        <button onClick={this.props.increment}>{this.props.likes}</button>
      </div>
    )
  }
}

function mapStateToProps(state){
  return {
    likes: state.likes
  }
}

export default connect(mapStateToProps, { increment })(Likes)
```


3. Having confirmed it was working I would then make all Logs responsible for managing their own likes in the API and persist their values server side:

```
# Actions.js - src/redux/modules/Logs/actions.js

  return dispatch => {
    const updatedLikes = log.likes + 1
    return fetch(`${API_URL}/logs/${log.id}`, {
      method: "PATCH",
      headers: {
        'Accept': 'application/json',
        'Content-Type': 'application/json'
      },
      body: JSON.stringify (
        {
          "log": { "likes": `${updatedLikes}` }
        }
      )
    })
    .then(response => response.json())
    .then(log => dispatch(updateLog(log)))
  }
  
# Reducer.js - src/redux/modules/Logs/reducer.js

const logsReducer = (state = [], action) => {
  switch(action.type) {
    ...
    case 'UPDATE_LOG_SUCCESS':
      const newState = state.map(l => {
        if (l.id === action.payload.id) {
          return action.payload
        } else {
          return l
        }
      })
      return newState

# Logs.js (Sort) - src/views/Logs/Logs.js

class Logs extends Component {

  componentDidMount() {
    this.props.getLogs()
  }

  render() {

    const logList = this.props.logs.map((log, index) =>
      <LogCard key={index} log={log} />
    )
    console.log(logList)

    return (
      <div>
        <h1 className="center">My Logs</h1>
        {logList.sort((a,b) => {
          return b.props.log.likes - a.props.log.likes
        })}
      </div>
    );
  }
}

const mapStateToProps = (state) => {
  return ({
    logs: state.logs
  })
}

export default connect(mapStateToProps, { getLogs })(Logs);
```


As always thank you for reading and if you have any questions about what I’ve done here, what I’m up to, or just generally want to chat feel free to reach out to me at:
 xseanclarke@gmail.com
