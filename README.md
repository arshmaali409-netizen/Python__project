# Python__project
# ADVANCED SCHOOL MANAGEMENT SYSTEM
# New additions: setter method, grade calculation, exception handling,
# a School class to manage many students, and saving records to a file.

class Person:                                        # PARENT class
    def __init__(self, name, marks):
        self.name = name
        self._roll_no = 101
        self.__marks = marks                          # private attribute

    def get_marks(self):                              # GETTER -> reads hidden data
        return self.__marks

    def set_marks(self, new_marks):                   # SETTER -> updates hidden data safely
        try:
            if 0 <= new_marks <= 100:
                self.__marks = new_marks
            else:
                raise ValueError("Marks must be between 0 and 100")
        except ValueError as e:
            print("Error:", e)

    def role(self):
        print(self.name, "is a Person")


class Student(Person):                                # SINGLE INHERITANCE
    def __init__(self, name, marks, student_id):
        super().__init__(name, marks)
        self.student_id = student_id

    def role(self):
        print(self.name, "is a Student, ID:", self.student_id)

    def calculate_grade(self):                        # NEW: uses the private data through get_marks()
        marks = self.get_marks()
        if marks >= 90:
            return "A+"
        elif marks >= 75:
            return "A"
        elif marks >= 60:
            return "B"
        elif marks >= 40:
            return "C"
        else:
            return "Fail"


class Topper(Student):                                # MULTILEVEL INHERITANCE
    def __init__(self, name, marks, student_id, rank):
        super().__init__(name, marks, student_id)
        self.rank = rank

    def role(self):
        print(self.name, "is a Topper, Rank:", self.rank)


class ScienceStudent(Student):                        # HIERARCHICAL INHERITANCE (child 1)
    def role(self):
        print(self.name, "is a Science Student")


class ArtStudent(Student):                            # HIERARCHICAL INHERITANCE (child 2)
    def role(self):
        print(self.name, "is an Art Student")


class Sportsman:
    def play(self):
        print("Playing in the school team")


class ChampionStudent(ScienceStudent, Sportsman):     # MULTIPLE INHERITANCE
    def role(self):
        print(self.name, "is a Champion Student")


class TopperAthlete(Topper, Sportsman):               # HYBRID INHERITANCE
    def role(self):
        print(self.name, "is a Topper Athlete")


def show_role(student):                               # POLYMORPHISM WITH FUNCTION AND OBJECTS
    student.role()


def show_all(students):                               # POLYMORPHISM WITH CLASS METHODS
    for s in students:
        s.role()


# ------------------------------------------------------------------
# NEW: A "School" class that manages a whole list of students
# ------------------------------------------------------------------
class School:
    def __init__(self, school_name):
        self.school_name = school_name
        self.students = []                             # keeps all student objects

    def add_student(self, student):
        self.students.append(student)
        print(student.name, "added to", self.school_name)

    def search_student(self, student_id):
        for s in self.students:
            if s.student_id == student_id:
                return s
        return None

    def show_report(self):
        print(f"\n--- {self.school_name} Report ---")
        for s in self.students:
            s.role()
            print("   Grade:", s.calculate_grade())

    def save_to_file(self, filename):                  # FILE HANDLING
        with open(filename, "w") as f:
            for s in self.students:
                f.write(f"{s.name}, ID: {s.student_id}, Marks: {s.get_marks()}, Grade: {s.calculate_grade()}\n")
        print("Records saved to", filename)


# ---------- DEMO ----------
s1 = ScienceStudent("Ali", 85, "S1")
s2 = ArtStudent("Sara", 78, "S2")
s3 = Topper("Bilal", 95, "S3", 1)
s4 = ChampionStudent("Ayesha", 90, "S4")
s5 = TopperAthlete("Hamza", 98, "S5", 2)

my_school = School("City Grammar School")
for student in (s1, s2, s3, s4, s5):
    my_school.add_student(student)

show_role(s1)
show_all((s1, s2, s3, s4, s5))

s4.play()
s5.play()

print("\nOriginal marks of Ali:", s1.get_marks())
s1.set_marks(150)                                       # invalid -> triggers exception handling
s1.set_marks(92)                                         # valid -> updates
print("Updated marks of Ali:", s1.get_marks())

found = my_school.search_student("S3")
if found:
    print("\nFound student:", found.name)

my_school.show_report()
my_school.save_to_file("students.txt")
