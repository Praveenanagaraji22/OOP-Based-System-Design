# OOP-Based-System-Design
# Objective
Design and simulate a course management system using core Object-Oriented Programming (OOP) principles in JavaScript.
# Features
1) User Roles: Student, Instructor
2) Course Creation by Instructor
3) Student Enrollment
4) Assignment Upload by Students
5) Assignment Grading by Instructor
6) Role-based functionality using polymorphism
# UML Class Diagram
<img width="927" height="1621" alt="ClassDiagram1" src="https://github.com/user-attachments/assets/7ea47de5-3188-4bf3-872e-47dc8bc4cc6f" />

# Implemention of the structure in JavaScript

```
class User {
  constructor(id, name, email) {
    this.id = id;
    this.name = name;
    this.email = email;
  }

  getRole() {
    return "User";
  }

  viewCourses() {
    console.log(`${this.name} is viewing available courses.`);
  }
}

class Student extends User {
  constructor(id, name, email) {
    super(id, name, email);
    this.enrolledCourses = [];
    this.grades = [];
  }

  enroll(course) {
    this.enrolledCourses.push(course);
    course.enrollStudent(this);
  }

  uploadAssignment(assignment, file) {
    assignment.submit(this, file);
  }

  getRole() {
    return "Student"; 
  }
}

class Instructor extends User {
  constructor(id, name, email) {
    super(id, name, email);
  }

  createCourse(courseId, title) {
    return new Course(courseId, title, this);
  }

  gradeAssignment(assignment, student, score) {
    const grade = new Grade(student, assignment, score);
    student.grades.push(grade);
  }

  getRole() {
    return "Instructor"; 
  }
}

class Course {
  constructor(courseId, title, instructor) {
    this.courseId = courseId;
    this.title = title;
    this.instructor = instructor;
    this.students = [];
    this.assignments = [];
  }

  enrollStudent(student) {
    this.students.push(student);
  }

  addAssignment(assignment) {
    this.assignments.push(assignment);
  }
}

class Assignment {
  constructor(assignmentId, title, content) {
    this.assignmentId = assignmentId;
    this.title = title;
    this.content = content;
    this.submissions = new Map(); 
  }

  submit(student, file) {
    this.submissions.set(student, file);
  }
}

class Grade {
  constructor(student, assignment, score) {
    this.student = student;
    this.assignment = assignment;
    this._score = score;
  }

  getScore() {
    return this._score;
  }

  setScore(score) {
    if (score >= 0 && score <= 100) {
      this._score = score;
    } else {
      console.log("Invalid score");
    }
  }
}

```

# OOP Design Explanation
1. Abstraction
Abstraction is used by creating classes that represent real-world entities like Student, Instructor, Course, Assignment, and Grade. Each class exposes only the relevant details. For instance:
The Student class includes methods like submitAssignment() or viewGrades() instead of revealing internal data structure.
Course encapsulates its own assignments, grades, and enrolled users abstractly.
This helps reduce complexity and makes the system easier to understand.

2. Encapsulation
Each class encapsulates its data by:
Declaring attributes like studentId, courseList, or assignmentTitle as private or protected.
Providing getters and setters to access or modify these attributes.
For example,
     the Grade class might allow getScore() and setScore() methods, but not expose the internal representation of grade calculation.
This ensures data security and integrity.

3. Inheritance
Inheritance is used to promote code reusability:
Both Student and Instructor could inherit from a common User or Person base class.
Shared attributes like name, email, or id are defined in the base class, and specific behaviors like submitAssignment() (for students) or gradeAssignment() (for instructors) are defined in their subclasses.

4. Polymorphism
Polymorphism is achieved by method overriding or interface implementation:
A base class or interface might define a method like displayProfile(), which is implemented differently in Student and Instructor.
This allows objects to be treated as instances of their parent class, improving flexibility and scalability.

# SOLID Principles Followed
## S – Single Responsibility Principle (SRP):
Each class has one responsibility:  
   Student handles student-specific logic.
   Course manages course-related info.
   Grade is responsible only for grading logic.

## O – Open/Closed Principle (OCP):
The system can be extended (e.g., adding a TA class or different Grade types) without modifying existing code.

## L – Liskov Substitution Principle (LSP):
Wherever a User object is expected, a Student or Instructor subclass can be substituted without breaking functionality.

# Conclusion
This project helped me understand and apply OOP concepts to build a basic course management system with real-world features like enrollment, assignment submission, and grading
