# Module12 -reflection

After module 12, implementing a JWT-based login and registration system helped me develop a deeper understanding of full-stack development. This module helped me explore how the frontend, backend, database, and authentication mechanisms work together to create a secure application. 

I also learned how client-side validation, such as checking email formats and password length, improve the user experience by providing immediate feedback. Besides that, I gained a better understanding of JWT storage on the client side and how tokens are included in requests to access protected resources. However, client-side validation cannot replace sever-side validation, password hashing, or token verification. 

One of the challenges I faced was configuring the Docker environment. Although my containers were running successfully, missing PostgreSQL tables caused authentication requests to return a 500 error. I resolved this issue by inspecting the database and creating the required tables. Another challenge I encountered was figuring out the PUT calculation endpoint. When I changed the calculation type from subtraction to division, the API response still displayed subtraction. After reviewing the code and module lecture video, I identified that the type field was missing from the update schema. I solved this issue by modifying a schema/calculation.py and adding the type field. 

Overall, this assignment strengthened my understanding of authentication, application security, automated testing, and API development while improve my ability to troubleshoot problem across different layers of a full-stack application. 
