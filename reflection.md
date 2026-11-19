# Module12 -reflection
After assignment12, implementing user registration, login, and calculation routes helped me understand how the models and database logic developed in Module 10 and 11 could be integrated into a functional RESTful API. Connecting FastAPI endpoints with SQLAlchemy models, the factory pattern, and polymorphic inheritance allowed me to better understand how different application component work together.

Integrating testing with pytest and Docker was particularly valuable in verifying endpoint reliability. Using a real PostgreSQL database in a containerized environment helped me understand how to validate database operations, HTTP status codes, and JSON responses. 

One challenge I faced was testing the PUT calculation endpoint. When I changed the calculation type from subtraction to division, the response still displayed subtraction. After reviewing the Pydantic schema and lecture video, I resolved the issue by modifying schema/calculation.py and adding the type field to the update schema. 

Overall, this assignment strengthen my understanding of RESTful API development, database integration, and application security. It also improved my troubleshooting skills and helped me connect concepts I learned from previous modules with practical backend development. 

