# Modules

In this exercise, you will refactor some code that manages student enrollment records for a workshop, to use the module pattern.

## Instructions

1. Wrap all of the functions in a module factory (ie, function named `defineWorkshop()`). This function should make a return a public API object.

2. The returned public API object should include the following methods:

   - `addStudent(id,name,paid)`
   - `enrollStudent(id)`
   - `printCurrentEnrollment()`
   - `enrollPaidStudents()`
   - `remindUnpaidStudents()`,

3. Move the `currentEnrollment` and `studentRecords` arrays inside the module definition, but as empty arrays.

4. Create an instance of this module by calling `defineWorkshop()`, and name it `deepJS`.

5. Define all the student records by calling `deepJS.addStudent(..)` for each.

6. Define the student enrollments by calling `deepJS.enrollStudent(..)` for each.

7. Change the execution code (the console output steps) to references to `deepJS.*` public API methods.

# Solution:

```JavaScript
var deepJS = defineWorkshop();

deepJS.addStudent(313,"Frank",/*paid=*/true);
deepJS.addStudent(410,"Suzy",/*paid=*/true);
deepJS.addStudent(709,"Brian",/*paid=*/false);
deepJS.addStudent(105,"Henry",/*paid=*/false);
deepJS.addStudent(502,"Mary",/*paid=*/true);
deepJS.addStudent(664,"Bob",/*paid=*/false);
deepJS.addStudent(250,"Peter",/*paid=*/true);
deepJS.addStudent(375,"Sarah",/*paid=*/true);
deepJS.addStudent(867,"Greg",/*paid=*/false);

deepJS.enrollStudent(410);
deepJS.enrollStudent(105);
deepJS.enrollStudent(664);
deepJS.enrollStudent(375);

deepJS.printCurrentEnrollment();
console.log("----");
deepJS.enrollPaidStudents();
console.log("----");
deepJS.remindUnpaidStudents();

/*
	Bob (664): Not Paid
	Henry (105): Not Paid
	Sarah (375): Paid
	Suzy (410): Paid
	----
	Bob (664): Not Paid
	Frank (313): Paid
	Henry (105): Not Paid
	Mary (502): Paid
	Peter (250): Paid
	Sarah (375): Paid
	Suzy (410): Paid
	----
	Bob (664): Not Paid
	Henry (105): Not Paid
*/

// ********************************

function defineWorkshop() {
    var currentEnrollment = [];
    var studentRecords = [];

    var publicAPI = {
        addStudent, 
        enrollStudent, 
        printCurrentEnrollment, 
        enrollPaidStudents, 
        remindUnpaidStudents
        };

    return publicAPI;
    
    // ********************************

    function addStudent(id, name, paid) {
        studentRecords.push({id, name, paid});
    }

    function enrollStudent(id) {
        if (!currentEnrollment.includes(id)) {
            currentEnrollment.push(id);
        }
    }

    function printCurrentEnrollment() {
        printRecords(currentEnrollment);
    }

    function enrollPaidStudents() {
        currentEnrollment = paidStudentsToEnroll();
        printRecords(currentEnrollment);
    }

    function remindUnpaidStudents() {
        remindUnpaid(currentEnrollment);
    }

    function getStudentById(studentId) {
        return studentRecords.find(matchId);

        // ******************

        function matchId(record) {
            return (record.id == studentId);
        }
    }

    function printRecords(recordIds) {
        var records = recordIds.map(getStudentById);

        records.sort(sortByNameAsc);

        records.forEach(printRecord);
    }

    function printRecord(record) {
        console.log(`${record.name} (${record.id}): ${record.paid ? "Paid": "Not Paid"}`);
    }

    function sortByNameAsc(record1, record2) {
        if (record1.name < record2.name) return -1;
        else if (record1.name > record2.name) return 1;
        else return 0;
    }

    function paidStudentsToEnroll() {
        var recordsToEnroll = studentRecords.filter(needToEnroll);

        var idsToEnroll = recordsToEnroll.map(getStudentId);

        return [...currentEnrollment , ...idsToEnroll];
    }

    function getStudentId(record) {
        return record.id;
    }

    function needToEnroll(record) {
        return (record.paid && !currentEnrollment.includes(record.id));
    }

    function remindUnpaid(recordIds) {
        var unpaidIds = recordIds.filter(isUnpaid);

        printRecords(unpaidIds);
    }

    function isUnpaid(studentId) {
        var record = getStudentById(studentId);
        return !record.paid;
    }
}
```
