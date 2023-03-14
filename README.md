# spritle-
regarding the problem
Backend:
The backend should have the following APIs:
Signup API: This API should allow users to create an account.
Login API: This API should allow users to log in.
Create Activity API: This API should allow the master to create a new activity.
Get Activities API: This API should allow the students to get a list of activities sought by the master.
Submit Activity API: This API should allow the students to submit the result of an activity.
Get Activity Log API: This API should allow the master to get a list of activities sought by them and their results.


from flask import Flask, jsonify, request
from flask_jwt_extended import (
    JWTManager, jwt_required, create_access_token, get_jwt_identity
)
import math

app = Flask(__name__)
app.config['JWT_SECRET_KEY'] = 'super-secret'  # Replace with a random string in production
jwt = JWTManager(app)

activities = []

# Signup API
@app.route('/signup', methods=['POST'])
def signup():
    # Create user account and save to database
    return jsonify({'message': 'User created successfully!'})

# Login API
@app.route('/login', methods=['POST'])
def login():
    email = request.json.get('email')
    password = request.json.get('password')
    
    # Authenticate user and generate access token
    access_token = create_access_token(identity=email)
    return jsonify({'access_token': access_token})

# Create Activity API
@app.route('/activity', methods=['POST'])
@jwt_required
def create_activity():
    if get_jwt_identity() != 'master@example.com':
        return jsonify({'message': 'Not authorized to create activities!'}), 401
    
    left_operand = int(request.json.get('left_operand'))
    right_operand = int(request.json.get('right_operand'))
    operator = request.json.get('operator')
    
    if operator == 'plus':
        result = left_operand + right_operand
    elif operator == 'minus':
        result = left_operand - right_operand
    elif operator == 'times':
        result = left_operand * right_operand
    elif operator == 'divided_by':
        result = math.floor(left_operand / right_operand)
    else:
        return jsonify({'message': 'Invalid operator!'}), 400
    
    activities.append({
        'left_operand': left_operand,


Frontend:
The frontend should have the following pages:
Login Page: This page should allow users to log in using their email and password.
Signup Page: This page should allow users to create an account by entering their email, password, and other details.
Dashboard Page: This page should display different options based on the user role. For students, it should display a list of activities sought by the master. For the master, it should allow them to create a new activity and see the results of activities.
Activity Log Page: This page should display a list of activities sought by the master and their results.


<!DOCTYPE html>
<html>
<head>
	<title>Math Activity Log</title>
</head>
<body>
	<h1>Math Activity Log</h1>
	
	<!-- Master Login Form -->
	<form id="master-login" style="display:none;">
		<h2>Master Login</h2>
		<label for="master-username">Username:</label>
		<input type="text" id="master-username" name="master-username"><br>
		<label for="master-password">Password:</label>
		<input type="password" id="master-password" name="master-password"><br>
		<button type="submit">Login</button>
	</form>
	
	<!-- Student Login Form -->
	<form id="student-login" style="display:none;">
		<h2>Student Login</h2>
		<label for="student-username">Username:</label>
		<input type="text" id="student-username" name="student-username"><br>
		<label for="student-password">Password:</label>
		<input type="password" id="student-password" name="student-password"><br>
		<button type="submit">Login</button>
	</form>
	
	<!-- Activity Log Table -->
	<table id="activity-log" style="display:none;">
		<thead>
			<tr>
				<th>Activity</th>
				<th>Result</th>
			</tr>
		</thead>
		<tbody>
			<!-- Activity log entries will be dynamically generated here -->
		</tbody>
	</table>
	
	<script>
		// Code for showing/hiding login forms and activity log
		const masterLoginForm = document.querySelector('#master-login');
		const studentLoginForm = document.querySelector('#student-login');
		const activityLog = document.querySelector('#activity-log');
		
		function showMasterLoginForm() {
			masterLoginForm.style.display = 'block';
			studentLoginForm.style.display = 'none';
			activityLog.style.display = 'none';
		}
		
		function showStudentLoginForm() {
			masterLoginForm.style.display = 'none';
			studentLoginForm.style.display = 'block';
			activityLog.style.display = 'none';
		}
		
		function showActivityLog() {
			masterLoginForm.style.display = 'none';
			studentLoginForm.style.display = 'none';
			activityLog.style.display = 'block';
		}
		
		// Code for handling form submissions and activity log updates
		const masterLoginButton = document.querySelector('#master-login button');
		const studentLoginButton = document.querySelector('#student-login button');
		
		masterLoginButton.addEventListener('click', function(event) {
			event.preventDefault();
			// Code for authenticating master user and showing activity log
			showActivityLog();
		});
		
		studentLoginButton.addEventListener('click', function(event) {
			event.preventDefault();
			// Code for authenticating student user and updating activity log
			const activityLogTable = document.querySelector('#activity-log tbody');
			activityLogTable.innerHTML = '';
			// Code for retrieving activity log entries from server and adding them to table
			showActivityLog();
		});
	</script>
</body>
</html>
