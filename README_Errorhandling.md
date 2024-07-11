# ERROR HANDLING (REQUIRE,ASSERT,REVERT)

This is a simple SchoolGradingSystem that performs the error handling using require,revert and assert method . It shouws the registration grade assign and about semester whether it ended or not.

## Description

It uses the 0.8.0 solidity version in which the contract is named as SchoolGradingSystem in which there are three function registered student, gradeassigned and get grade. which perform different tasks using require revert and assert methods.

## Getting Started


### Executing program
To run this program , i am using Remix , an Solidity IDE.
->To start go to Remix IDE and start coding 
->add a solidity file i named it as schoolgrading.sol
->include the licence (SPDX) 
->use the latest solidity compiler by including the pragma which will compiler the code using that solidity compiler
->make a contract named SchoolGradingSystem 
->under that contract create three function named registerStudent, assignGrade, getGrade which take different parameters as address,name,grade,subjects. Under these function the require ,revert and assert methods are called to check the condition and revert the message on the screen.
->click on compile (compile schoolgrading.sol)
->go to deploy and deploy it.
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SchoolGradingSystem {
    address public administrator;
    bool public semesterEnded = false;

    struct Student {
        string name;
        mapping(string => uint8) grades; // subject => grade
    }

    mapping(address => Student) public students;

    event StudentRegistered(address studentAddress, string studentName);
    event GradeAssigned(address studentAddress, string subject, uint8 grade);
    event SemesterEnded();

    modifier onlyAdmin() {
        require(msg.sender == administrator, "Only the administrator can perform this action");
        _;
    }

    modifier semesterActive() {
        require(!semesterEnded, "Semester has already ended");
        _;
    }

    constructor() {
        administrator = msg.sender;
    }

    function registerStudent(address _studentAddress, string memory _studentName) public onlyAdmin semesterActive {
        Student storage student = students[_studentAddress];
        require(bytes(student.name).length == 0, "Student already registered");

        student.name = _studentName;
        emit StudentRegistered(_studentAddress, _studentName);
    }

    function assignGrade(address _studentAddress, string memory _subject, uint8 _grade) public onlyAdmin semesterActive {
        require(_grade <= 100, "Grade must be between 0 and 100");

        Student storage student = students[_studentAddress];
        require(bytes(student.name).length != 0, "Student not registered");

        student.grades[_subject] = _grade;
        emit GradeAssigned(_studentAddress, _subject, _grade);
    }

    function getGrade(address _studentAddress, string memory _subject) public view returns (uint8) {
        Student storage student = students[_studentAddress];
        require(bytes(student.name).length != 0, "Student not registered");

        return student.grades[_subject];
    }

    function endSemester() public onlyAdmin {
        semesterEnded = true;
        emit SemesterEnded();
        assert(semesterEnded == true);
    }

    fallback() external payable {
        revert("Fallback function called, action not allowed");
    }

    receive() external payable {
        revert("Direct transfers not allowed");
    }
}

```

## Help

Change the compiler solidity version if error occur , then change by going to compiler solidity menu present on left side of the compiler and change the compiler version.

## Authors

Amrita Kumari
->https://www.linkedin.com/in/amrita-kumari-753319232/


## License

TThis project is licensed under the MIT License - see the licence file for details.
