// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.7;

contract EmploymentSystem {
    address public employer;
    
    struct Employee {
        string name;
        uint salary;
        bool isActive;
    }

    mapping(address => Employee) public employees;

    event EmployeeAdded(address indexed employeeAddress, string name, uint salary);
    event SalarySet(address indexed employeeAddress, uint newSalary);
    event SalaryPaid(address indexed employeeAddress, uint amount);

    modifier onlyEmployer() {
        require(msg.sender == employer, "Only the employer can execute this");
        _;
    }

    constructor() {
        employer = msg.sender;
    }

    function addEmployee(address employeeAddress, string memory name, uint salary) external onlyEmployer {
        require(!employees[employeeAddress].isActive, "Employee already exists");

        Employee memory newEmployee = Employee({
            name: name,
            salary: salary,
            isActive: true
        });

        employees[employeeAddress] = newEmployee;

        emit EmployeeAdded(employeeAddress, name, salary);
    }

    function getEmployee(address employeeAddress) external view returns (string memory, uint, bool) {
        Employee memory employee = employees[employeeAddress];
        return (employee.name, employee.salary, employee.isActive);
    }

    function setSalary(address employeeAddress, uint newSalary) external onlyEmployer {
        require(employees[employeeAddress].isActive, "Employee does not exist");

        employees[employeeAddress].salary = newSalary;

        emit SalarySet(employeeAddress, newSalary);
    }

    function paySalary(address employeeAddress) external onlyEmployer {
        require(employees[employeeAddress].isActive, "Employee does not exist");

        uint salaryAmount = employees[employeeAddress].salary;
        require(address(this).balance >= salaryAmount, "Insufficient contract funds");

        employees[employeeAddress].isActive = false;

        payable(employeeAddress).transfer(salaryAmount);

        emit SalaryPaid(employeeAddress, salaryAmount);
    }

    receive() external payable {}

    // uint public balanceReceived;

    // function receiveSalary() public payable {
    //     balanceReceived +=msg.value;
    // }

    // function getBalance() public view returns (uint) {
    //     return address (this).balance;
    // }

    // function withdrawSalary() public payable {
    //     address payable to = payable (msg.sender);
    //     to.transfer(getBalance());
    // }
}