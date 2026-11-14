# Lab 1: Knowledge-Based Systems

This folder contains the practical work and code for Lab 1, including the Knowledge Base and Inference Engine.

## Part 2: Hands-On Student Exercises

This section includes the implementation of both exercises completed as part of the lab practice.

---

# Exercise 1: Degree Honors Rule

**Task:** Add a new rule to the Knowledge Base for students completing their degree program.

**Conditions:**
- CGPA must be 3.70 or above.
- Disciplinary warnings must be zero.

**Expected Status:** Graduated with Magna Cum Laude Honors.

## Step 1: Add a New Degree Honors Rule

**Explanation:** Add `RULE_07` to the Knowledge Base with the required CGPA and disciplinary warning conditions.

**Code:**

```python
UNIVERSITY_KB["rules"].append({
    "id": "RULE_07",
    "condition_type": "degree_honors",
    "conditions": {
        "cgpa_min": 3.70,
        "disciplinary_warnings": 0
    },
    "status": "Graduated with Magna Cum Laude Honors",
    "action": "Eligible for graduation honors."
})

print("Degree honors rule added successfully!")
```

**Output:**

```text
Degree honors rule added successfully!
```

## Step 2: Create the Degree Honors Evaluation Function

**Explanation:** This function checks whether the student meets both degree honors conditions and returns the appropriate result.

**Code:**

```python
def evaluate_degree_honors(student, kb):

    rule = next(
        (r for r in kb["rules"] if r["id"] == "RULE_07"),
        None
    )

    if rule is None:
        return {
            "rule_id": "UNKNOWN",
            "status": "Rule Not Found",
            "action": "Add RULE_07 to the Knowledge Base."
        }

    conditions = rule["conditions"]

    if (
        student["cgpa"] >= conditions["cgpa_min"]
        and student["disciplinary_warnings"] == conditions["disciplinary_warnings"]
    ):
        return {
            "rule_id": rule["id"],
            "status": rule["status"],
            "action": rule["action"]
        }

    return {
        "rule_id": rule["id"],
        "status": "Not Eligible for Magna Cum Laude Honors",
        "action": "The student does not meet all honors requirements."
    }


print("Degree honors evaluation function defined successfully!")
```

**Output:**

```text
Degree honors evaluation function defined successfully!
```

## Step 3: Test the Degree Honors Function

**Explanation:** Create a student record and evaluate eligibility using the degree honors function.

**Code:**

```python
graduate_student = {
    "student_id": "BSCS-2024-012",
    "student_name": "Ali Khan",
    "cgpa": 3.85,
    "disciplinary_warnings": 0
}

result = evaluate_degree_honors(graduate_student, UNIVERSITY_KB)

print("Student:", graduate_student["student_name"])
print("Rule ID:", result["rule_id"])
print("Status:", result["status"])
print("Action:", result["action"])
```

**Output:**

```text
Student: Ali Khan
Rule ID: RULE_07
Status: Graduated with Magna Cum Laude Honors
Action: Eligible for graduation honors.
```

**Result:** The system successfully applied `RULE_07`. Ali Khan meets both conditions and is eligible for Magna Cum Laude Honors.

---

# Exercise 2: Tracing Forward Chaining

**Objective:** To modify the inference engine to display each rule condition during evaluation and demonstrate how the system reaches a conclusion using Forward Chaining.

## Step 1: Modify the Inference Engine

**Explanation:** Modify the `evaluate_student()` function to display each rule condition and its result (`TRUE` or `FALSE`). The system first checks attendance and then evaluates marks-based rules.

**Code:**

```python
def evaluate_student(student_record, kb):

    print(f"\n--- EVALUATING STUDENT: {student_record['student_name']} ---")

    attendance = student_record["attendance"]
    marks = student_record["total_marks"]

    # Check Attendance Rule

    attendance_rule = next(
        rule for rule in kb["rules"]
        if rule.get("condition_type") == "attendance_below"
    )

    print("\nChecking RULE_01: Attendance Constraint")
    print(f"Condition: {attendance} < 75")

    if attendance < 75:
        print("Result: TRUE")
        print("Conclusion: Student is debarred.")

        return {
            "rule_id": attendance_rule["id"],
            "grade": attendance_rule["grade"],
            "status": attendance_rule["status"],
            "action": attendance_rule["action"]
        }

    print("Result: FALSE")
    print("Moving to marks-based rules.")

    # Check Marks-Based Rules

    for rule in kb["rules"]:

        if rule.get("condition_type") != "marks_range":
            continue

        minimum = rule["conditions"]["marks_min"]
        maximum = rule["conditions"]["marks_max"]

        print(f"\nChecking {rule['id']}: Marks Range")
        print(f"Condition: {minimum} <= {marks} <= {maximum}")

        if minimum <= marks <= maximum:
            print("Result: TRUE")
            print(f"Conclusion: Grade {rule['grade']}")

            return {
                "rule_id": rule["id"],
                "grade": rule["grade"],
                "status": rule["status"],
                "action": rule["action"]
            }

        print("Result: FALSE")
        print("Moving to the next rule.")

    # Handle Invalid Marks

    print("\nNo grading rule matched.")

    return {
        "rule_id": "UNKNOWN",
        "grade": "N/A",
        "status": "Invalid Score Entry",
        "action": "Verify submitted marks with the exam department."
    }


print("Forward Chaining Inference Engine Defined Successfully!")
```

**Output:**

```text
Forward Chaining Inference Engine Defined Successfully!
```

## Step 2: Create Student Records

**Explanation:** Create two student records with different marks and attendance values to test the inference engine.

**Code:**

```python
student_1 = {
    "student_id": "BSCS-2024-012",
    "student_name": "Ali Khan",
    "total_marks": 88,
    "attendance": 92
}

student_2 = {
    "student_id": "BSCS-2024-045",
    "student_name": "Sara Ahmed",
    "total_marks": 81,
    "attendance": 68
}

print("Student records created successfully!")
```

**Output:**

```text
Student records created successfully!
```

## Step 3: Test Forward Chaining for Ali Khan

**Explanation:** Evaluate Ali Khan's record. His attendance is above 75%, so the system moves to marks-based rules and checks which rule matches his marks.

**Code:**

```python
result_1 = evaluate_student(student_1, UNIVERSITY_KB)

print("\n========== FINAL RESULT: ALI KHAN ==========")
print("Applied Rule:", result_1["rule_id"])
print("Grade:", result_1["grade"])
print("Status:", result_1["status"])
print("Advisory Action:", result_1["action"])
```

**Output:**

```text
--- EVALUATING STUDENT: Ali Khan ---

Checking RULE_01: Attendance Constraint
Condition: 92 < 75
Result: FALSE
Moving to marks-based rules.

Checking RULE_02: Marks Range
Condition: 85 <= 88 <= 100
Result: TRUE
Conclusion: Grade A

========== FINAL RESULT: ALI KHAN ==========
Applied Rule: RULE_02
Grade: A
Status: Passed with Distinction
Advisory Action: Recommended for Merit Scholarship
```

**Result:** The system applied `RULE_02` and assigned Grade A to Ali Khan because his marks fall within the required range.

## Step 4: Test Forward Chaining for Sara Ahmed

**Explanation:** Evaluate Sara Ahmed's record. Her attendance is below 75%, so the system applies `RULE_01` and marks her as debarred.

**Code:**

```python
result_2 = evaluate_student(student_2, UNIVERSITY_KB)

print("\n========== FINAL RESULT: SARA AHMED ==========")
print("Applied Rule:", result_2["rule_id"])
print("Grade:", result_2["grade"])
print("Status:", result_2["status"])
print("Advisory Action:", result_2["action"])
```

**Output:**

```text
--- EVALUATING STUDENT: Sara Ahmed ---

Checking RULE_01: Attendance Constraint
Condition: 68 < 75
Result: TRUE
Conclusion: Student is debarred.

========== FINAL RESULT: SARA AHMED ==========
Applied Rule: RULE_01
Grade: F
Status: Debarred
Advisory Action: Ineligible to sit for final exam due to short attendance.
```

**Result:** The system applied `RULE_01` because Sara Ahmed's attendance is below 75%, making her ineligible to sit for the final exam.

---

# Conclusion

Both exercises were completed successfully:

- **Exercise 1:** Added a new degree honors rule to the Knowledge Base to check student eligibility.
- **Exercise 2:** Updated the Inference Engine to show how rules are checked step by step and how the system reaches a decision.
