---
layout: post
title:  "Javascript Promises: Pinky’s Up"
date:   2017-09-27 21:44:56 +0000
---

[](http://i.imgur.com/BjHPVhr.png)

A **promise** is an object that produces a value in the future upon the completion of (more often than not) an api call, or some other supplied argument. At any given time, a promise is either either completed, rejected, or pending. Through callbacks we can utilize the completed value or provide feedback for what it may not have been provided.
For instance, in my most recent project, JetLog, I utilized promises extensively in handling and dealing with JSON data from my companion Rails API. As seen below, in my Redux Logs module, I make an asynchronous call to my API to **fetch** “logs”. Using **.then()**, I’m able to promise the function that something will be done with the data retrieved.!

```
// ** Async Actions **

export const getLogs = () => {
  return dispatch => {
    return fetch(`${API_URL}/logs`)
    .then(response => response.json())
    .then(logs => dispatch(setLogs(logs)))
    .catch(err => {
      throw new SubmissionError(err)
    })
  }
}
```

In this case, I’m…
**fetching** a response and ensuring it’s delivered as **json**
taking the logs array and **dispatching a callback** of **setlogs(logs)**, passing in the logs array [See Fig 2.].
in the event there’s an error I deploy a **.catch** and alert of a Submission Error.
If successful, the setlogs(logs) action will switch the reducer and update the logs state to reflect all the logs gathered from the call.

```
// action 
const setLogs = logs => {
  return {
    type: 'GET_LOGS_SUCCESS',
    logs
  }
}
// reducer
const logsReducer = (state = [], action) => {
  switch(action.type) {
    case 'GET_LOGS_SUCCESS':
      return action.logs;
  ...}
...}
```

