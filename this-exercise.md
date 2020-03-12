# `this`

In this exercise, you will refactor some code that manages student enrollment records for a workshop, from the module pattern to the namespace pattern using the `this` keyword.

## Instructions

1. Remove the `defineWorkshop()` module factory, and replace it with an object literal (named `deepJS`) that holds all the module's functions, as well as the `currentEnrollment` and `studentRecords` data arrays.

2. Change all internal function references and references to the data  arrays to use the `this` keyword prefix.

3. Make sure any place where a `this`-aware callback is passed is hard-bound with `bind(..)`. Don't `bind(..)` a function reference if it's not `this`-aware.

# Solution:

```JavaScript
var deepJS = {
    currentEnrollment: [],
    studentRecords: [],
    addStudent(id, name, paid) {
        this.studentRecords.push({id, name, paid});
    },
    enrollStudent(id) {
        if (!this.currentEnrollment.includes(id)) {
            this.currentEnrollment.push(id);
        }
    },
    printCurrentEnrollment() {
        this.printRecords(this.currentEnrollment);
    },
    enrollPaidStudents() {
        this.currentEnrollment = this.paidStudentsToEnroll();
        this.printCurrentEnrollment();
    },
    remindUnpaidStudents() {
        this.remindUnpaid(this.currentEnrollment);
    },
    getStudentById(studentId) {
        return this.studentRecords.find(matchId);

        // ******************

        function matchId(record) {
            return (record.id == studentId);
        }
    },
    printRecords(recordIds) {
        var records = recordIds.map(this.getStudentById.bind(this));

        records.sort(this.sortByNameAsc);

        records.forEach(this.printRecord);
    },
    sortByNameAsc(record1, record2) {
        if (record1.name < record2.name) return -1;
        else if (record1.name > record2.name) return 1;
        else return 0;
    },
    printRecord(record) {
        console.log(`${record.name} (${record.id}): ${record.paid ? "Paid": "Not Paid"}`);
    },
    paidStudentsToEnroll() {
        var recordsToEnroll = this.studentRecords.filter(this.needToEnroll.bind(this));

        var idsToEnroll = recordsToEnroll.map(this.getStudentId);

        return [...this.currentEnrollment , ...idsToEnroll];
    },
    needToEnroll(record) {
        return (record.paid && !this.currentEnrollment.includes(record.id));
    },
    getStudentId(record) {
        return record.id;
    },
    remindUnpaid(recordIds) {
        var unpaidIds = recordIds.filter(this.isUnpaid.bind(deepJS));

        this.printRecords(unpaidIds);
    },
    isUnpaid(studentId) {
        var record = this.getStudentById(studentId);
        return !record.paid;
    }
};

// ********************************

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
```