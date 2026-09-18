# hotel-management-system
#include <iostream>
#include <fstream>
#include <vector>
#include <string>
#include <iomanip>

using namespace std;

// ======================================================
// Customer Class
// ======================================================
class Customer {
private:
    int customerId;
    string name;
    string phone;

public:
    Customer() {
        customerId = 0;
    }

    Customer(int id, string n, string p) {
        customerId = id;
        name = n;
        phone = p;
    }

    int getCustomerId() const {
        return customerId;
    }

    string getName() const {
        return name;
    }

    string getPhone() const {
        return phone;
    }

    void display() const {
        cout << left
             << setw(12) << customerId
             << setw(25) << name
             << setw(15) << phone << endl;
    }
};

// ======================================================
// Room Class
// ======================================================
class Room {
private:
    int roomNumber;
    string roomType;
    double price;
    bool booked;
    int customerId;

public:
    Room() {
        roomNumber = 0;
        price = 0;
        booked = false;
        customerId = 0;
    }

    Room(int number, string type, double p) {
        roomNumber = number;
        roomType = type;
        price = p;
        booked = false;
        customerId = 0;
    }

    int getRoomNumber() const {
        return roomNumber;
    }

    string getRoomType() const {
        return roomType;
    }

    double getPrice() const {
        return price;
    }

    bool isBooked() const {
        return booked;
    }

    int getCustomerId() const {
        return customerId;
    }

    // Book the room
    bool bookRoom(int id) {
        if (booked) {
            return false;   // Prevent double booking
        }

        booked = true;
        customerId = id;
        return true;
    }

    // Checkout the room
    bool checkout() {
        if (!booked) {
            return false;
        }

        booked = false;
        customerId = 0;
        return true;
    }

    // Used when loading saved data from file
    void setBookingStatus(bool status, int id) {
        booked = status;
        customerId = id;
    }

    void display() const {
        cout << left
             << setw(12) << roomNumber
             << setw(15) << roomType
             << setw(12) << fixed << setprecision(2) << price
             << setw(15) << (booked ? "Booked" : "Available")
             << endl;
    }
};

// ======================================================
// Hotel Management System Class
// ======================================================
class HotelManagementSystem {
private:
    vector<Room> rooms;
    vector<Customer> customers;

    const string ROOM_FILE = "rooms.txt";
    const string CUSTOMER_FILE = "customers.txt";

public:

    // --------------------------------------------------
    // Initialize Rooms
    // --------------------------------------------------
    void initializeRooms() {

        // Add rooms only if no rooms exist
        if (!rooms.empty())
            return;

        rooms.push_back(Room(101, "Single", 1500));
        rooms.push_back(Room(102, "Single", 1500));
        rooms.push_back(Room(103, "Double", 2500));
        rooms.push_back(Room(104, "Double", 2500));
        rooms.push_back(Room(105, "Deluxe", 3500));
        rooms.push_back(Room(106, "Deluxe", 3500));
        rooms.push_back(Room(107, "Suite", 5000));
        rooms.push_back(Room(108, "Suite", 5000));
    }

    // --------------------------------------------------
    // Find Room
    // --------------------------------------------------
    Room* findRoom(int roomNumber) {

        for (auto &room : rooms) {
            if (room.getRoomNumber() == roomNumber) {
                return &room;
            }
        }

        return nullptr;
    }

    // --------------------------------------------------
    // Find Customer
    // --------------------------------------------------
    Customer* findCustomer(int customerId) {

        for (auto &customer : customers) {
            if (customer.getCustomerId() == customerId) {
                return &customer;
            }
        }

        return nullptr;
    }

    // --------------------------------------------------
    // Search Room
    // --------------------------------------------------
    void searchRoom() {

        string type;

        cout << "\nEnter room type to search: ";
        cin >> type;

        bool found = false;

        cout << "\nAvailable " << type << " Rooms:\n";

        cout << left
             << setw(12) << "Room No."
             << setw(15) << "Type"
             << setw(12) << "Price"
             << setw(15) << "Status" << endl;

        cout << string(54, '-') << endl;

        for (const auto &room : rooms) {

            if (room.getRoomType() == type &&
                !room.isBooked()) {

                room.display();
                found = true;
            }
        }

        if (!found) {
            cout << "No available rooms found.\n";
        }
    }

    // --------------------------------------------------
    // Display All Rooms
    // --------------------------------------------------
    void displayRooms() {

        cout << "\n================ ROOM LIST ================\n";

        cout << left
             << setw(12) << "Room No."
             << setw(15) << "Type"
             << setw(12) << "Price"
             << setw(15) << "Status" << endl;

        cout << string(54, '-') << endl;

        for (const auto &room : rooms
