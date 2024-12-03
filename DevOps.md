# DevOps
## Waterfall Model
- It is similar to a waterfall. Once the water has flown to the edge of the cliff, it cannot turn back.
- The same is the case for the Waterfall Development Strategy as well.
- An application will go to the next stage only when the previous stage is complete.
  ![image](https://github.com/user-attachments/assets/b62a3dd0-bc3c-4c05-b331-02d425aec777)

**Limitations of Waterfall Model**
- Once an application is in testing stage, it is very difficult to go back and change something.
- High amounts of risk

## Agile Methodology
- Each project is broken up into several iterations.
- All iterations should be of the same time duration (2-8 weeks).
- At the end of each iteration, a working product should be delivered.
![image](https://github.com/user-attachments/assets/7c7dc032-2295-4295-9f1f-a0f9935b5525)

**Limitations of Agile**
- Disparity between the Developer team and the Operations team.

The solution to this is **DevOps**
DevOps bridges the gap between the Dev side and the Ops side of the company.
- DevOps is a methodology.
![image](https://github.com/user-attachments/assets/e09e9014-cf23-49b9-800b-9513bf5c3ea7)
![image](https://github.com/user-attachments/assets/cc8359d7-8719-4818-bfeb-6242deb972a3)
**Continuous Integration** : Building your application continuously. If any developer makes a change in the source code, a Continuous Integration (CI) server should be able to pull that code and prepare a build.
"prepare a build" here means to compile, validate, review code, unit test it, and integration test it.
**Continuous Delivery** : The build will then be deployed to test servers and do its testing.
**Continuous Deployment** (Not preferred): It will then be deployed on the production server for release. Here, we will use Configuration Management and Containerization tools.  

![image](https://github.com/user-attachments/assets/9cff83a3-b8b8-4e14-8b2b-b61b35aaba4b)

**Disadvantages of Centralized Version Control System**
- Not locally available. You will always have to connect to a network to take any action.
- If somehow the server crashes, the entire data will be lost.

 In Distributed Version Control System, every contributer has a local copy/clone of the main repository. https://www.youtube.com/watch?v=hQcFE0RD0cQ 19:33 

