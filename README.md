#include <iostream>
#include <vector>
#include <fstream>
#include <string> #include <algorithm>
using namespace std; 

class Student { public: 
    string id;     string fullName;     string contactEmail;     vector<string> coursesEnrolled; 

    Student(string student_id, string student_name, string student_email) {         id = student_id;         fullName = student_name; 
        contactEmail = student_email; 
    } 

    void registerCourse(const string& courseCode) {         if (find(coursesEnrolled.begin(), coursesEnrolled.end(), courseCode) 
== coursesEnrolled.end()) { 
coursesEnrolled.push_back(courseCode);             cout<< fullName << " registered for course: " << courseCode 
<<endl; 
        } else { 
cout<< "Already registered for " << courseCode <<endl; 
        } 
    } 

void unregisterCourse(const string& courseCode) { 
        auto pos = find(coursesEnrolled.begin(), coursesEnrolled.end(), courseCode); 
        if (pos != coursesEnrolled.end()) {             coursesEnrolled.erase(pos); 
cout<< fullName << " unregistered from course: " << courseCode <<endl; 
        } else { 
cout<< "Not registered in " << courseCode <<endl; 
        } 
    } 


    void displayCourses() const {         cout<< "\n" << fullName << "'s Enrolled Courses:\n"; 
        for (const auto& c : coursesEnrolled) {             cout<< "• " << c << '\n'; 
        } 
    } 
}; 

class Teacher { public: 
    string id; 
    string fullName;     string contactEmail; 
vector<string> coursesAssigned; 

    Teacher(string teacher_id, string teacher_name, string teacher_email) {         id = teacher_id;         fullName = teacher_name; 
        contactEmail = teacher_email; 
    } 


    void assignCourse(const string& courseCode) { 
        coursesAssigned.push_back(courseCode); 
    } 


    void removeCourse(const string& courseCode) { 
        auto pos = find(coursesAssigned.begin(), coursesAssigned.end(), courseCode); 
        if (pos != coursesAssigned.end()) { 
            coursesAssigned.erase(pos); 
        } 
    } 

        void displayCourses() const { 
cout<< "\nCourses taught by " << fullName << ":\n"; 
        for (const auto& c : coursesAssigned) {             cout<< "• " << c << '\n'; 
        } 
    } 
}; 

class Course { public: 
    string code; 
    string title; 
    string assignedTeacherID;     vector<string> studentIDs; 
    int capacity; 

    // Initialize course 
Course(string course_code, string course_title, string teacher_id, int max_students) {         code = course_code; 
        title = course_title;         assignedTeacherID = teacher_id; 
        capacity = max_students; 
    } 

void addStudent(const string& studentID) {         if ((int)studentIDs.size() < capacity) {             studentIDs.push_back(studentID);             cout<< "Student " << studentID << " enrolled in " << code <<endl; 
        } else { 
cout<< "Enrollment failed. Course is at full capacity.\n";         } 
    } 

    void removeStudent(const string& studentID) { 
        auto pos = find(studentIDs.begin(), studentIDs.end(), studentID);         if (pos != studentIDs.end()) {             studentIDs.erase(pos);             cout<< "Student " << studentID << " removed from " << code 
<<endl; 
        } 
    } 

    // Show all enrolled students     void displayStudents() const {         cout<< "\nStudents registered in " << title << " (" << code <<
"):\n";         for (const auto&sid : studentIDs) { 
cout<< "• " <<sid<< '\n'; 
        } 
    } }; 
void saveStudent(const Student& s) {     ofstream file("students.txt", ios::app);     file << s.id << "," << s.fullName << "," << s.contactEmail;     for (const auto& c : s.coursesEnrolled) { 
        file << "," << c; 
    }     file << '\n';     file.close(); 
} 


void saveTeacher(const Teacher& t) {     ofstream file("teachers.txt", ios::app);     file << t.id << "," << t.fullName << "," << t.contactEmail;     for (const auto& c : t.coursesAssigned) { 
        file << "," << c; 
    }     file << '\n';     file.close(); 
} 

// Save course data to file void saveCourse(const Course& c) {     ofstream file("courses.txt", ios::app);     file << c.code << "," << c.title << "," << c.assignedTeacherID << "," 
<< c.capacity; 
    for (const auto&sid : c.studentIDs) { 
        file << "," <<sid; 
    }     file << '\n';     file.close(); 
} 

int main() { 
    Student student1("A001", "Abdullah", "aryan@example.com"); 
    Teacher teacher1("A002", "Ali", "ali@university.edu"); 
Course course1("AI101", "Intro to AI", teacher1.id, 2); 


    teacher1.assignCourse(course1.code);     course1.addStudent(student1.id); 
    student1.registerCourse(course1.code); 

    student1.displayCourses();     teacher1.displayCourses();     course1.displayStudents();     saveStudent(student1);     saveTeacher(teacher1); 
    saveCourse(course1); 

    return 0; } 
