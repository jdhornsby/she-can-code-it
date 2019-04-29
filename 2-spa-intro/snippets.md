# Helpful snippets

Sample buildspec - dev:
```
version: 0.2

env:
  variables:
    REACT_APP_BACKEND_URL: "https://wvvmujiev0.execute-api.us-east-1.amazonaws.com/dev"
phases:
  install:
    commands:
       - cd front-end
       - npm ci
  pre_build:
    commands:
       - CI=true npm test
  build:
    commands:
       - npm run build
artifacts:
  files:
     - '**/*'
  base-directory: front-end/build
```

App.js
```
import React, {Component} from 'react';
import './App.css';

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      posts: []
    }
  }
  
  componentDidMount() {
    fetch(`${process.env.REACT_APP_BACKEND_URL}/api/v1/posts`)
      .then(response => response.json())
      .then(data => this.setState({ posts: data.posts }));
  }
  
  render() {
    const {posts} = this.state;
    return (
      <div>
        <ul>
         {posts.map(post =>
          <li key={post.id}>
            <div>
              <b>{post.subject}</b>
            </div>
            <div>
              {post.message}
            </div>
            <div>
              Posted by {post.createdBy} at {post.createdAt}
            </div>
          </li>
        )}
        </ul>
      </div>
    );
  }
}

export default App;
```

handler.js
```
'use strict';

module.exports.getPosts = async (event) => {
  return {
    statusCode: 200,
    headers: {
      'Access-Control-Allow-Origin': '*'
    },
    body: JSON.stringify({
      posts: [{
        id: 1,
        createdAt: new Date(),
        createdBy: 'Josh',
        subject: 'Hello world',
        message: 'My first fake post!!!'
      }]  
    })
  };
};
```