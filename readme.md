To-Do List for Timetable Web Application Using AWS
1. Set Up EC2 Instance

    Launch an EC2 instance with a security group configured to allow traffic on the following ports:
        22 (SSH)
        80 (HTTP)
        443 (HTTPS)
        5432 (PostgreSQL)
        8000 (Flask app)
    Ensure all inbound rules allow traffic from the required IP addresses or CIDR blocks.

2. Set Up RDS PostgreSQL

    Create an RDS PostgreSQL instance with public access enabled.
    Configure the RDS security group to allow inbound traffic from the EC2 instance security group (use its ID).
    Note the following details for connection:
        <RDS_End_Point> (RDS endpoint)
        <RDS_User> (RDS username)
        <RDS_Database_Name> (Database name)

3. SSH into EC2

    Connect to the EC2 instance using SSH:

    ssh -i "C:\path_to_your_key.pem" ubuntu@<EC2_Public_IP>

4. Update EC2 and Install Dependencies

    Update and install the necessary packages:

    sudo apt update
    sudo apt install -y python3 python3-pip postgresql-client

5. Connect to RDS PostgreSQL

    Verify the connection to your RDS database using the psql command:

    psql -h <RDS_End_Point> -U <RDS_User> -d <RDS_Database_Name>

6. Create and Populate the Database

    Inside the psql console, create a timetable table:

CREATE TABLE timetable (
    id SERIAL PRIMARY KEY,
    course_name VARCHAR(100),
    level VARCHAR(20),
    day VARCHAR(20),
    time VARCHAR(20)
);

Insert sample data:

    INSERT INTO timetable (course_name, level, day, time)
    VALUES 
        ('Global Social Problems', 'Undergraduate', 'Monday', '10:00 AM'),
        ('Computer Languages', 'Undergraduate', 'Tuesday', '12:00 PM'),
        ('Social Engineering and Society', 'Graduate', 'Wednesday', '2:00 PM'),
        ('Introduction to Ethics', 'Undergraduate', 'Thursday', '4:00 PM');

7. Set Up Flask Application

    Create a project directory:

mkdir my_project
cd my_project
touch app.py

Edit app.py to include the Flask application with PostgreSQL connectivity. Below is a sample skeleton:

    from flask import Flask, render_template
    import psycopg2

    app = Flask(__name__)

    def get_timetable():
        conn = psycopg2.connect(
            host='<RDS_End_Point>',
            database='<RDS_Database_Name>',
            user='<RDS_User>',
            password='<RDS_Password>'
        )
        cur = conn.cursor()
        cur.execute("SELECT course_name, level, day, time FROM timetable")
        rows = cur.fetchall()
        cur.close()
        conn.close()
        return rows

    @app.route('/')
    def index():
        timetable = get_timetable()
        return render_template('index.html', timetable=timetable)

    if __name__ == '__main__':
        app.run(host='0.0.0.0', port=8000)

8. Create index.html Template

    Create the templates directory and index.html:

mkdir templates
touch templates/index.html

Add the following content to index.html:

    <!DOCTYPE html>
    <html>
    <head>
        <title>Timetable</title>
    </head>
    <body>
        <h1>Course Timetable</h1>
        <table border="1">
            <tr>
                <th>Course Name</th>
                <th>Level</th>
                <th>Day</th>
                <th>Time</th>
            </tr>
            {% for course in timetable %}
            <tr>
                <td>{{ course[0] }}</td>
                <td>{{ course[1] }}</td>
                <td>{{ course[2] }}</td>
                <td>{{ course[3] }}</td>
            </tr>
            {% endfor %}
        </table>
    </body>
    </html>

9. Install Python Dependencies

    Install required libraries:

    sudo pip3 install --upgrade pip
    pip3 install flask psycopg2-binary

10. Set Up Virtual Environment

    Create and activate a Python virtual environment:

    python3 -m venv venv
    source venv/bin/activate

11. Disable Firewall (Optional)

    If required, disable the firewall to allow incoming traffic:

    sudo ufw disable

12. Run the Flask Application

    Start the Flask app:

    python app.py

13. Access the Application

    Open your web browser and navigate to:

http://<EC2_Public_IP>:8000

![изображение](https://github.com/user-attachments/assets/168041ca-8084-49b4-8832-740a6fe6be71)

![изображение](https://github.com/user-attachments/assets/145be5a1-8b9f-4455-938e-1cc9a2f69bab)

