---
layout: project
type: project
image: img/hotelmanagement/hotel-management-system.jpg
title: "Hotel Management System"
date: 2023-12-10
published: true
labels:
  - VS Code
  - Replit
  - C++
summary: "My team and I created a basic hotel management system in the language of C++ for a final project, displaying our capabilities and skills from the course."
---

&nbsp;

### Project Overview

<img width="250px" 
     class="rounded float-start pe-4" 
     src="../img/hotelmanagement/hotel-managementstock.jpeg" >

  The Hotel Management System Project was a final project for the course ICS 212 in which we had to construct a basic hotel management syetm in C++. The class was divided into teams of 5 or 6 and we were all responsible for programming different sections of the code and then putting the program together. I took the lead in this project, by guiding my team and fine-tuning the program. My team and I used cstring arrays, structures, and class derivation to achieve this goal. We went through several different versions of the program, in which we were encountering bugs in our original implementation of a vector aproach. I took on the role of debugging the majority of the program, and realized that a cstring approach would be much more manageable for the build. Using the framework of the original code, we developed a fully functioning hotel management system that would store user input, rely on a main menu with sub-menus present based on the user's input, and the ability to combine multiple system options into one displayable output.

Here is the README file for the project that details the group members, what each group member's tasks were, and an overview of the code. As you can see, my tasks for this project included creating the Hotel class, the room mangagement system functions, creating a way for users to search for a customer within the system, and debugging the whole system.

```txt
Group 6 Hotel Management System

Group Members:

Zai 
John
Kayla
JaySuh
T

Member Tasks:

John - CheckIn, CheckOut, getSummaryReport, debugging

JaySuh - Customer class, delete room, debugging

Zai - Room Class, addRoom, available rooms, debugging

Kayla - switch case statements, sub and main menu, debugging

T - Hotel class, room management system functions, searchCustomer, debugging

Code Overview:

- Room class containing room details about room #, ac, comfort, etc. using constructors

- Room details displayed through  function

- Customer class conatainging and aligning with customer details (name, address, phone, stay duration, payment, etc.). Getter function for getting the booking ID by name for case 4. 

- Customer details displayed through function

- Hotel Management class inheriting from Room class using customer and room details, room management system, customer search, check-in check-out (payment calculations), availability, summary report, and menu display functions

- main function containing do while loop for menu using switch case statement for options prompted through user input

- idea for further iteration: make checkOut take checkIn and checkOut dates to automatically calculate bill without user input

- avail rooms issue fixed, originally would iterate empty inputs, possibly due to input of enter for next records, had trouble trying to fix but is all working now

- deleteRooms function fixed, would just pop back to main menu

- other functionality and fundamental issues fixed using time given till Sunday
```

&nbsp;

Upon running the program users would be presented with this menu within the console:

&nbsp;

######## Hotel Management #########

1. Manage Rooms
2. Check-In Room
3. Available Rooms
4. Search Customer
5. Check-Out Room
6. Guest Summary Report
7. Exit

Enter Option: 

&nbsp;

Here, they would enter one of the main menu options and follow the subsequential prompts for each. From there, they would also be able to return to this main menu at any time.

&nbsp;

We developed the system within VS Code as such:

<img width="700px" height="500px"
     class="rounded float-center pe-4" 
     src="../img/hotelmanagement/Screen Shot 2024-05-09 at 1.33.44 PM.png" >

&nbsp;

### Learning Outcomes

  This project helped me to learn a lot, mainly when working in a collaborative and professional setting. Since each member of the team had to contribute various parts of the code for it then to be put together greatly reflected a real-world setting in a programming environment. I feel that this project not only tested my skills but also gave me insight in regards to the industries synonymous with programming. The experience also provided me with a great oppurunity to develop and improve my own leadership skills. It also helped to learn more about the different functionalities specifically within C++ and how they can be applied within both fictional and real world settings, all while offering intellectual challenges in the process of collaborative group work.

&nbsp;

Here is the full code that my team and I developed for the project:

```cpp
#include <iostream>
#include <string>
#include <cstring> //cstring helped fix issues of vector original

#define max 100 //prevents exceeding

class Customer { //class customer with customer detail strings, float for adv, and int for id
public:
    std::string name;
    std::string address;
    std::string phone;
    std::string from_date;
    std::string to_date;
    float payment_advance;
    int booking_id;
}; 

class Room { //class room for room details, status, and creates array for rooms and keeps count
public:
    char type;
    char stype;
    char ac;
    int roomNumber;
    int rent;
    int status;
    static int count; //uses static count to keep track of # of rooms
    static Room rooms[max]; //array for rooms stored in system

    Room() : status(0) {} //constructor to initialize status for rooms

    Customer cust; //used for customer detail calls and displays

    //uses int for search with room #
    static Room addRoom(int);
    void searchRoom(int);
    void deleteRoom(int);
    static void displayRoom(const Room&);
};

int Room::count = 0; //initializes static count
Room Room::rooms[max]; //initializes static array

void manageRooms() {
    int opt, rno, i, flag = 0; //option, room #, flag for t/f
    char ch;
    do {
        std::cout << "\n### Manage Rooms ###";
        std::cout << "\n1. Add Room";
        std::cout << "\n2. Search Room";
        std::cout << "\n3. Delete Room";
        std::cout << "\n4. Back to Main Menu";
        std::cout << "\n\nEnter Option: ";
        std::cin >> opt; //takes in option for menu

        //switch case for room management menu
        switch (opt) {
            case 1:
                std::cout << "\nEnter Room Number: ";
                std::cin >> rno; //takes in room #
                i = 0;
                for (i = 0; i < Room::count; i++) {
                    if (Room::rooms[i].roomNumber == rno) { //room # to array
                        flag = 1; //flag to 1
                    }
                }
                if (flag == 1) {
                    std::cout << "\nRoom Number is Present.\nPlease enter a unique Number";
                    flag = 0; //if room already exists
                } else {
                    Room::rooms[Room::count] = Room::addRoom(rno); //adds room through function call
                }
                break;

            case 2:
                std::cout << "\nEnter room number: ";
                std::cin >> rno;
                Room::rooms[0].searchRoom(rno); //searches by room # through function call
                break;

            case 3:
                std::cout << "\nEnter room number to delete: ";
                std::cin >> rno;
                Room::rooms[0].deleteRoom(rno); //removes from array by room # through function call
                break;

            case 4:
                //used to go back to the main menu
                return;

            default:
                std::cout << "\nPlease Enter the correct option";
                break; //if incorrect input
        }
    } while (true);
}


Room Room::addRoom(int rno) { //addRoom function for room details
    Room room;
    room.roomNumber = rno;
    std::cout << "\nType AC/Non-AC (A/N) : ";
    std::cin >> room.ac;
    std::cout << "\nType Comfort (S/N) : ";
    std::cin >> room.type;
    std::cout << "\nType Size (B/S) : ";
    std::cin >> room.stype;
    std::cout << "\nDaily Rent : ";
    std::cin >> room.rent;
    ++Room::count; //updates static count
    Room::rooms[Room::count - 1] = room; //adds new room to array
    std::cout << "\n Room Added Successfully!";
    return room;
}

void Room::searchRoom(int rno) { //searches and displays room details if present
    int i, found = 0;
    for (i = 0; i < Room::count; i++) {
        if (Room::rooms[i].roomNumber == rno) {
            found = 1;
            break;
        }
    }

    if (found == 1) {
        std::cout << "Room Details\n";
        if (Room::rooms[i].status == 1) { //for reserved or open
            std::cout << "\nRoom is Reserved";
        } else {
            std::cout << "\nRoom is available";
        }
        displayRoom(Room::rooms[i]);
    } else {
        std::cout << "\nRoom not found"; //incorrect input
    }
}

void Room::deleteRoom(int rno) { //delets room from array if present through input rno
    int i, found = 0;
    for (i = 0; i < Room::count; i++) {
        if (Room::rooms[i].roomNumber == rno) {
            found = 1;
            break;
        }
    }
    if (found == 1) {
        //shifts remaining rooms to fill gap
        for (int j = i; j < Room::count - 1; j++) {
            Room::rooms[j] = Room::rooms[j + 1];
        }
        --Room::count; //updates static count
        std::cout << "\nRoom Deleted Successfully!";
    } else {
        std::cout << "\nRoom not found";
    }
}

void Room::displayRoom(const Room &tempRoom) { //displays room details using tempRoom
    std::cout << "\nRoom Number: \t" << tempRoom.roomNumber;
    std::cout << "\nType AC/Non-AC (A/N) " << tempRoom.ac;
    std::cout << "\nType Comfort (S/N) " << tempRoom.type;
    std::cout << "\nType Size (B/S) " << tempRoom.stype;
    std::cout << "\nRent: " << tempRoom.rent;
}

class HotelMgnt : protected Room { //hm class inheriting from room
private:
    int found;

public: //public void functions for management system
    void checkIn();
    void getAvailRoom();
    void searchCustomer(const char *);
    void checkOut(int);
    void guestSummaryReport();
};

void HotelMgnt::guestSummaryReport() {
    if (Room::count == 0) { //for if none entered (first option choice)
        std::cout << "\n No Guest in Hotel !!";
    }
    for (int i = 0; i < Room::count; i++) { //if guest ecists, displays details entered from check-in
        if (Room::rooms[i].status == 1) {
            std::cout << "\n Customer First Name : " << Room::rooms[i].cust.name;
            std::cout << "\n Room Number : " << Room::rooms[i].roomNumber;
            std::cout << "\n Address (only city) : " << Room::rooms[i].cust.address;
            std::cout << "\n Phone : " << Room::rooms[i].cust.phone;
            std::cout << "\n---------------------------------------";
        }
    }
}

void HotelMgnt::checkIn() { //checks in guest to room
    int i, found = 0, rno;

    Room room;
    std::cout << "\nEnter Room number : ";
    std::cin >> rno;
    for (i = 0; i < Room::count; i++) {
        if (Room::rooms[i].roomNumber == rno) {
            found = 1;
            break;
        }
    }

    if (found == 1) {
        if (Room::rooms[i].status == 1) { //if room taken by another guest
            std::cout << "\nRoom is already Booked";
            return;
        }

        std::cout << "\nEnter booking id: "; //id assigned to customer
        std::cin >> Room::rooms[i].cust.booking_id;

        std::cout << "\nEnter Customer Name (First Name): ";
        std::cin >> Room::rooms[i].cust.name; //name assigned

        std::cout << "\nEnter Address (only city): ";
        std::cin >> Room::rooms[i].cust.address; //city assigned

        std::cout << "\nEnter Phone: ";
        std::cin >> Room::rooms[i].cust.phone; //phone assigned

        std::cout << "\nEnter From Date: ";
        std::cin >> Room::rooms[i].cust.from_date; 
        //dates staying (used in calcuating total bill at end)
        std::cout << "\nEnter to  Date: ";
        std::cin >> Room::rooms[i].cust.to_date;

        std::cout << "\nEnter Advance Payment: ";
        std::cin >> Room::rooms[i].cust.payment_advance; //takes in and deducts advance from total bill

        Room::rooms[i].status = 1; //updates roo status to occupied

        std::cout << "\n Customer Checked-in Successfully..";
    }
}

void HotelMgnt::getAvailRoom() { //used to display the rooms that have been entered and aren't taken
    int i, found = 0;
    for (i = 0; i < Room::count; i++) {
        if (Room::rooms[i].status == 0) { //relies on room status
            Room::displayRoom(Room::rooms[i]);
            std::cout << "\n\nPress enter for next room";
            found = 1;
        }
    }
    if (found == 0) { //if all taken
        std::cout << "\nAll rooms are reserved";
    }
}

void HotelMgnt::searchCustomer(const char *pname) { //searched customer using ptr
    int i, found = 0;
    for (i = 0; i < Room::count; i++) {
        if (Room::rooms[i].status == 1 && strcmp(Room::rooms[i].cust.name.c_str(), pname) == 0) {
            //string comparison to see if matches
            std::cout << "\nCustomer Name: " << Room::rooms[i].cust.name;
            std::cout << "\nRoom Number: " << Room::rooms[i].roomNumber;
            std::cout << "\nAddress: " << Room::rooms[i].cust.address;
            std::cout << "\nPhone Number: " << Room::rooms[i].cust.phone;
            std::cout << "\n\nPress enter for next record\n";
            //presents customer details entered from check-in prompt
            found = 1;
        }
    }
    if (found == 0) { //person not entered or incorrect name input
        std::cout << "\nPerson not found.\n";
    }
}

void HotelMgnt::checkOut(int roomNum) { //used to check out guest and displays $
    int i, found = 0, days;
    float billAmount = 0; //initializes bill
    for (i = 0; i < Room::count; i++) {
        if (Room::rooms[i].status == 1 && Room::rooms[i].roomNumber == roomNum) {
            found = 1;
            break;
        }
    }

    if (found == 1) {
        std::cout << "\nEnter Number of Days:\t";
        std::cin >> days;
        billAmount = days * Room::rooms[i].rent; //calcs bill based on days and rent

        std::cout << "\n\t######## CheckOut Details ########\n";
        std::cout << "\nCustomer Name : " << Room::rooms[i].cust.name;
        std::cout << "\nRoom Number : " << Room::rooms[i].roomNumber;
        std::cout << "\nAddress : " << Room::rooms[i].cust.address;
        std::cout << "\nPhone : " << Room::rooms[i].cust.phone;
        std::cout << "\nTotal Amount Due : " << billAmount << " /"; //displays total before adv added
        std::cout << "\nAdvance Paid: " << Room::rooms[i].cust.payment_advance << " /";
        //outputs the total payable bill after deducting the advance payment from total amount
        std::cout << "\n*** Total Payable: " << billAmount - Room::rooms[i].cust.payment_advance << "/ only";

        Room::rooms[i].status = 0; //updates room status to unoccupied
    }
}

int main() {
    int opt, rno;
    char pname[100]; //char name array
    HotelMgnt hm; //used to call in hm class functions

    do { //do while statement for main menu
        std::cout << "######## Hotel Management #########\n";
        std::cout << "\n1. Manage Rooms";
        std::cout << "\n2. Check-In Room";
        std::cout << "\n3. Available Rooms";
        std::cout << "\n4. Search Customer";
        std::cout << "\n5. Check-Out Room";
        std::cout << "\n6. Guest Summary Report";
        std::cout << "\n7. Exit";
        std::cout << "\n\nEnter Option: ";
        std::cin >> opt;

        switch (opt) {
            case 1:
                manageRooms(); //calls manage rooms menu and associated functions
                break;

            case 2:
                if (Room::count == 0) { //if no rooms added upon startup
                    std::cout << "\nRooms data is not available.\nPlease add the rooms first.";
                } else {
                    hm.checkIn(); //prompts check-in if rooms have been entered
                }
                break;

            case 3:
                if (Room::count == 0) {
                    std::cout << "\nRooms data is not available.\nPlease add the rooms first.";
                } else {
                    hm.getAvailRoom(); //displays avail rooms
                }
                break;

            case 4:
                if (Room::count == 0) { 
                    std::cout << "\nRooms are not available.\nPlease add the rooms first.";
                } else {
                    std::cout << "Enter Customer Name: ";
                    std::cin >> pname; //takes in customer name
                    hm.searchCustomer(pname); //for search opt
                }
                break;

            case 5:
                if (Room::count == 0) {
                    std::cout << "\nRooms are not available.\nPlease add the rooms first.";
                } else {
                    std::cout << "Enter Room Number : ";
                    std::cin >> rno; //takes in room #
                    hm.checkOut(rno); //used to see who is checking out and what room
                }
                break;

            case 6:
                hm.guestSummaryReport(); //displays guest details and report
                break;

            case 7: //exits hm system
                std::cout << "\nTHANK YOU! FOR USING SOFTWARE\n";
                break;

            default: //if proper input not provided
                std::cout << "\nPlease Enter correct option";
                break;
        }
    } while (opt != 7); //so long as 7 not entered, repeats

    return 0;
}
```

(Unfortunately, the path to the original GitHub repository from the course is privated and no longer accessible.)

