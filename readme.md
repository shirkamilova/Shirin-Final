# aws_mini_project
Project to create simple webpage to check timetable use resources using recourse from AWS

Here's the revised to-do list with placeholders for the RDS connection details:

1. **Set up EC2 Instance**:
   - Configure the security group to allow traffic on ports: `80`, `22`, `443`, `5432`, `8000`, and `all traffic`.

2. **Set up RDS PostgreSQL**:
   - Make sure the RDS instance allows all traffic from the EC2 instance (public access).

3. **SSH into EC2**:
   - Run:  
     ```bash
     ssh -i "C:\your_key_2_ec2.pem" ubuntu@<EC2_Public_IP>
     ```

4. **Update EC2 and Install Dependencies**:
   - Run the following commands to update and install necessary packages:
     ```bash
     sudo apt update
     sudo apt install python3 python3-pip postgresql-client -y
     ```

5. **Connect to RDS PostgreSQL**:
   - Connect to the RDS instance using psql:
     ```bash
     psql -h <RDS_End_Point> -U <RDS_User> -d <RDS_Database_Name>
     ```

6. **Create and Populate Database Table**:
   - Create the `timetable` table:
     ```sql
     CREATE TABLE timetable (
         id SERIAL PRIMARY KEY,
         course_name VARCHAR(100),
         level VARCHAR(20),
         day VARCHAR(20),
         time VARCHAR(20)
     );
     ```
   - Insert sample course data:
     ```sql
     
INSERT INTO timetable (course_name, level, day, time)
VALUES
    ('Global Social Problems', 'Undergraduate', 'Monday', '10:00 AM'),
    ('Computer Languages', 'Undergraduate', 'Tuesday', '12:00 PM'),
    ('Social Engineering and Society', 'Graduate', 'Wednesday', '2:00 PM'),
    (' Introduction to Ethics', 'Undergraduate', 'Thursday', '4:00 PM');
     ```

7. **Create Project Directory and Flask Application**:
   - Create the project directory and Python file:
     ```bash
     mkdir my_project
     cd my_project
     touch app.py
     ```
   - Edit `app.py` and implement the Flask application with PostgreSQL connection.

8. **Create `index.html` Template**:
   - Create and populate `index.html` to display the timetable.

9. **Install Python Dependencies**:
   - Install the necessary libraries:
     ```bash
     pip3 install --upgrade pip
     pip3 install flask pg8000 psycopg2-binary
     ```

10. **Set Up Python Virtual Environment**:
    - Set up and activate the virtual environment:
      ```bash
      python3 -m venv venv
      source venv/bin/activate
      ```

11. **Disable Firewall (Optional)**:
    - If needed, disable the firewall:
      ```bash
      sudo ufw disable
      ```

12. **Run Flask Application**:
    - Run the Flask app:
      ```bash
      python app.py
      ```

13. **Access the Application**:
    - Open the browser and access the web application on your EC2 instance at `http://<EC2_Public_IP>:8000`.

This sequence now includes placeholders (`<RDS_End_Point>`, `<RDS_User>`, `<RDS_Database_Name>`, `<EC2_Public_IP>`) to replace with the actual connection details for your EC2 and RDS instances.
