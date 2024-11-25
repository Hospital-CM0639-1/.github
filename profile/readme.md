# Hospital
This is only as a grouping repository for other repos
- no code should be here
- https://excalidraw.com/#room=754f6ec9bd1616873aec,vsKPkodWg41pWdWQYwVqSA
![alt text](image-1.png)

Client:
- UI for users, with multiple roles
- API - proxy, between UI and Services, also caching and fallback

Services:
- core functionality of our system
- Management
  - administration of our hospital
- Reception 
  - administration for patients
  - separeted from management for better scalability
- Emergency
  - tool for emergency
- Doctors
  - tools for doctors 
- Cronjobs
  - tool for scheduled workers 

Database
- one database with master-slave topology
