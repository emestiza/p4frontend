# Project Overview

## Project Links

Link to completed project [here](https://emp4.netlify.app/#/).
Link to project frontend [here](https://github.com/emestiza/p4frontend).
Link to project backend [here](https://github.com/emestiza/p4backend).

## Project Schedule

|  Day | Deliverable | Status
|---|---| ---|
|Day 1| Project Description | Complete
|Day 1| Wireframes / Priority Matrix / Timeline `backend` and `frontend`| Complete
|Day 2| Working RestAPI | Complete
|Day 3| Core Application Structure (HTML, CSS, Vue.js) | Complete
|Day 4| Core Application Structure (Vue.js) | Complete
|Day 5| MVP & Bug Fixes | Complete
|Day 6| Final Touches | Complete

## Project Description

The goal of this project is to build an information database full stack application for school and studying puroposes. The API in the backend is built using Python, Django, and PSQL. The API has three models and two associations. The user model is associated to the subject model and the subject model is associated to the topic model. There is Create, Read, Update, and Destroy (CRUD) functionality built throughout the application models. In the frontend JavaScript and Vue.js are used to connect and leverage the backend API. While HTML and CSS are used for styling and organization purposes. The fronend also has a responsive design for mobile, tablet, and desktops. 

## Wireframes

Below are links to wireframes that show the application blueprint for mobile, tablet, and desktop display sizes.

- [Mobile](https://res.cloudinary.com/dssciwyew/image/upload/v1599931278/Mobile%20P4.png)
- [Tablet](https://res.cloudinary.com/dssciwyew/image/upload/v1599931278/Tablet%20P4.png)
- [Desktop](https://res.cloudinary.com/dssciwyew/image/upload/v1599931278/Desktop%20P4.png)

## Time/Priority Matrix 

Here is a full list of features that have been [prioritized](https://res.cloudinary.com/dssciwyew/image/upload/v1599935146/Priority%20Matrix%20Frontend%20P4.png) using a `Time and Priority` Matix. 

### MVP/PostMVP

The functionality will then be divided into two separate lists: MPV and PostMVP.  The contents in the MVP list are expect to be implemented and functioning upon project completion.  

#### MVP

- Navigation bar
- Log in form
- Sign up form
- Data input forms 
- CRUD functionality
- Vue.js
- Responsive design

#### PostMVP 

- Multiple pages
- Login page
- Footer

## Functional Components

Based on the initial logic defined in the previous sections, the logic is broken down further into functional components.

#### MVP
| Component | Priority | Estimated Time | Time Invetsted | Actual Time |
| --- | :---: |  :---: | :---: | :---: |
| Navigation bar | M | 2hrs | 2hrs | 2hrs|
| Log in form | H | 2hrs | 2hrs | 2hrs|
| Sign up form | H | 2hrs | 2hrs | 2hrs|
| Data input forms | H | 2hrs| 2hrs | 2hrs |
| CRUD functionality | H | 8hrs | 8hrs | 8hrs|
| Vue.js R&D | H | 8hrs| 8hrs | 8hrs |
| Responsive design | M | 1hr | 1hr | 1hr|
| Total | H | 25hrs| 25hrs | 25hrs |

#### PostMVP
| Component | Priority | Estimated Time | Time Invetsted | Actual Time |
| --- | :---: |  :---: | :---: | :---: |
| Multiple pages | L | 4hrs | -4hrs | -4hrs|
| Login page | L | 2hrs | -0hrs | -0hrs|
| Footer | L | 1hr | 1hr | 1hr|
| Total | H | 7hrs| 5hrs | 5hrs |

## Additional Libraries
The Buefy library was used for responsive UI components for Vue.js based on Bulma framework and design. 

## Code Snippet

This is a brief code snippet of sign up functionality. Here a component for new user sign up is created. The registration url is fetched and then posts 
the user input as JSON. If the sign up is completed successfully, then the user will be redirected to the log in page.  

```
export default {
  name: "Signup",
  data: function() {
    return {
      first_name: "",
      last_name: "",
      username: "",
      email: "",
      password: ""
    };
  },
  methods: {
    handleSignup: function() {
      console.log(this.$route.query.URL);
      fetch(`${this.$route.query.URL}/auth/users/register/`, {
        method: "post",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify({
          first_name: this.first_name,
          last_name: this.last_name,
          username: this.username,
          email: this.email,
          password: this.password
        })
      }).then(response => {
        if (response.status == 201) {
          response.json().then(data => {
            console.log(data);
            this.$router.push({
              name: "Login",
              query: this.$route.query,
              params: { message: "Sign up complete! Log in to continue." }
            });
          });
        } else {
          return response.json();
        }
      });
    }
  }
};
```

## Issues and Resolutions
**ERROR**: [Vue warn]: Error in created hook: "TypeError: Cannot read property 'token' of undefined"                               
**RESOLUTION**: Share token between pages. Added the following to Header.vue: 
```
<router-link :to="{name:'Main', query:{URL:this.URL, tokens:this.$attrs.info}}">Main</router-link>
```

