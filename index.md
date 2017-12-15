---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: home
---

We want you to like your pantry. We want managing it to be fun. No more forgotten ingredients; find a cool recipe for them instead. The back of your fridge doesn't need to be a scary place. Saveory was made for that. It was made to be scalable, independent, and free. Our journey begins and ends with the platform. One that is elaborate and cutting-edge but also friendly and easy to pickup. This is a story about developing microservices in the cloud. The IBM cloud.

### GitHub
- {LINK TO ORG}

### Technology Pit Stop
- Saveory is developed with `IBM Microservice Builder` and `NodeJS`.
- Saveory builds with `Docker`.
- Saveory deploys with the continuous integration `IBM Microservice Builder Pipeline`.****
- Saveory runs on Kubernetes. We prefer `IBM Cloud Private`.

### Being RESTful 4 services later
#### A. Palatable Pantries
- Start with a better way to keep track of ingredients. Just went to the store? There's an endpoint to add to your virtual pantry. Want to update your pantry to reflect ingredient usage in a recipe? We got you covered. Don't take my word for it, here's our RESTful HTTP API: [Swagger API](/swagger)
- ###### Built with IBM Microservice Builder

#### B. Remixed Recipes
- Ready to find recipes that prioritize using the stuff you already have? Look no further. Get only the best options that work with your dietary restrictions.
- ###### Built with IBM Microservice Builder

#### C. 'Umble Users
- Save your preferences. Change your password. It's all here.
- ###### Built with IBM Microservice Builder

#### D. Flexible Frontend
- The interace that ties it all together. See ingredients that are expiring soon or search for that sweet recipe. 		
- ###### Built with ReactJS on NodeJS

### Awesome! How can I use/develop/test Saveory?
- Saveory is an open source application with 4 services
         All 4 services build into Docker containers and can be run in a Kubernetes environment.

#### **Setup/Installation**:

- Follow the 3 steps in the [IBM Microservice Builder webpage](https://developer.ibm.com/microservice-builder/#getStarted). After following these steps you should have Docker, Git and the Bluemix CLI installed on your system.
- Now you would need to fork the Saveory repositories (Guide on how to do it: https://help.github.com/articles/fork-a-repo/ and Organization link: {LINK TO ORG} ) into your account and then clone that fork into your system.

#### **Development/Integration**:

*For our deployment management, we decided to use IBM Cloud Private for company sampling purposes but you can use other ones like Kubernetes (which is almost equivalent), Amazon ECS, Docker Swarm, etc.*

- We suggest using Kubernetes to deploy your project since it comes already configured to work with it, although you may utilize any other deployment management tool if needed.

- After having you’re Kubernetes environment, you can configure a pipeline to be able to automatically deploy updated versions of the application due to repository update detection.  

**See the repositories for each service for specific instructions**
1. {LINKS TO REPOS HERE}
