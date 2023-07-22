from flask import Flask, render_template, request, redirect, url_for, Response
import csv
import io

app = Flask(__name__)

# Replace this with your database logic (e.g., SQLite, JSON, etc.)
# For simplicity, we'll use some dummy data as examples.
students = [
    {"id": 1, "name": "John Doe", "college": "ABC College", "status": "placed", "dsaScore": 90, "webdScore": 85, "reactScore": 95},
    {"id": 2, "name": "Jane Smith", "college": "XYZ College", "status": "not_placed", "dsaScore": 80, "webdScore": 75, "reactScore": 85},
    # Add more dummy data here...
]

interviews = [
    {"id": 1, "company": "TechCorp", "date": "2023-07-22"},
    {"id": 2, "company": "WebTech", "date": "2023-07-25"},
    # Add more dummy data here...
]

results = [
    {"student_id": 1, "interview_id": 1, "result": "PASS"},
    # Add more dummy data here...
]

# Define a global variable to track the student IDs
current_student_id = 3  # Update with the current maximum ID + 1

# Define a global variable to track the interview IDs
current_interview_id = 3  # Update with the current maximum ID + 1

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/students')
def student_list():
    return render_template('student_list.html', students=students)

@app.route('/students/new', methods=['GET', 'POST'])
def add_student():
    global current_student_id

    # Handle new student form submission here
    if request.method == 'POST':
        name = request.form['name']
        college = request.form['college']
        status = request.form['status']
        dsa_score = int(request.form['dsa_score'])
        webd_score = int(request.form['webd_score'])
        react_score = int(request.form['react_score'])

        # Assign a unique ID to the new student
        new_student_id = current_student_id
        current_student_id += 1

        # Add the new student to the students list
        students.append({
            "id": new_student_id,
            "name": name,
            "college": college,
            "status": status,
            "dsaScore": dsa_score,
            "webdScore": webd_score,
            "reactScore": react_score
        })

        # Redirect to the student list page after adding the student
        return redirect(url_for('student_list'))

    return render_template('student_form.html')

@app.route('/interviews')
def interview_list():
    return render_template('interview_list.html', interviews=interviews)

@app.route('/interviews/new', methods=['GET', 'POST'])
def add_interview():
    global current_interview_id

    # Handle interview form submission here
    if request.method == 'POST':
        company = request.form['company']
        date = request.form['date']

        # Assign a unique ID to the new interview
        new_interview_id = current_interview_id
        current_interview_id += 1

        # Add the new interview to the interviews list
        interviews.append({"id": new_interview_id, "company": company, "date": date})

        # Redirect to the interview list page after adding the interview
        return redirect(url_for('interview_list'))

    return render_template('interview_form.html')

@app.route('/students/<int:student_id>/update_result', methods=['POST'])
def update_interview_result(student_id):
    result = request.form.get('result', "")

    # Find the student in the students list
    student = next((s for s in students if s["id"] == student_id), None)
    if not student:
        # Redirect to student list if student ID is not found
        return redirect(url_for('student_list'))

    # Update the student's interview result in the results list
    result_data = next((r for r in results if r["student_id"] == student_id), None)
    if not result_data:
        results.append({"student_id": student_id, "result": result})
    else:
        result_data["result"] = result

    # Redirect back to the student details page
    return redirect(url_for('view_student', student_id=student_id))

@app.route('/students/<int:student_id>')
def view_student(student_id):
    student = next((s for s in students if s["id"] == student_id), None)
    if not student:
        # Redirect to student list if student ID is not found
        return redirect(url_for('student_list'))

    # Find the corresponding interview data for the student
    interview_data = next((i for i in interviews if i["id"] == student_id), {})
    student["interview_date"] = interview_data.get("date", "")
    student["interview_company"] = interview_data.get("company", "")

    # Find the corresponding interview result for the student
    interview_result_data = next((r for r in results if r["student_id"] == student_id), {})
    student["interview_result"] = interview_result_data.get("result", "")

    return render_template('view_student.html', student=student)

@app.route('/students/<int:student_id>/edit', methods=['GET', 'POST'])
def edit_student(student_id):
    student = next((s for s in students if s["id"] == student_id), None)
    if not student:
        # Redirect to student list if student ID is not found
        return redirect(url_for('student_list'))

    # Handle student edit form submission here
    if request.method == 'POST':
        student['name'] = request.form['name']
        student['college'] = request.form['college']
        student['status'] = request.form['status']
        student['dsaScore'] = int(request.form['dsa_score'])
        student['webdScore'] = int(request.form['webd_score'])
        student['reactScore'] = int(request.form['react_score'])

        # Redirect to the student details page after editing
        return redirect(url_for('view_student', student_id=student_id))

    return render_template('edit_student.html', student=student)


@app.route('/download_csv')
def download_csv():
    # Create a CSV string from the data
    csv_data = io.StringIO()
    csv_writer = csv.writer(csv_data)
    csv_writer.writerow(["Student id", "Name", "College", "Status", "DSA Final Score", "WebD Final Score", "React Final Score", "Interview Date", "Interview Company", "Interview Result"])

    for student in students:
        student_id = student["id"]
        name = student["name"]
        college = student["college"]
        status = student["status"]
        dsa_score = student["dsaScore"]
        webd_score = student["webdScore"]
        react_score = student["reactScore"]

        interview_data = next((i for i in interviews if i["id"] == student_id), {})
        interview_date = interview_data.get("date", "")
        interview_company = interview_data.get("company", "")
        interview_result_data = next((r for r in results if r["student_id"] == student_id), {})
        interview_result = interview_result_data.get("result", "")

        csv_writer.writerow([student_id, name, college, status, dsa_score, webd_score, react_score, interview_date, interview_company, interview_result])

    # Create the CSV response and set the appropriate headers
    response = Response(csv_data.getvalue(), mimetype='text/csv')
    response.headers["Content-Disposition"] = "attachment; filename=student_interview_data.csv"

    return response

if __name__ == '__main__':
    app.run(debug=True)
