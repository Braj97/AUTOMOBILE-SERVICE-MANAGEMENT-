# AUTOMOBILE-SERVICE-MANAGEMENT-
C++
#include <iostream>
#include <fstream>
#include <iomanip>
#include <cstring>

using namespace std;

class Vehicle {
private:
    int jobID;
    char ownerName[50];
    char vehicleType[20];
    char vehicleNumber[15];
    char serviceType[30];
    float serviceCost;
    float spareCost;
    float totalCost;

public:
    void input() {
        cout << "\nEnter Job ID: ";
        cin >> jobID;
        cin.ignore();

        cout << "Owner Name: ";
        cin.getline(ownerName, 50);

        cout << "Vehicle Type (Car/Bike/Truck): ";
        cin.getline(vehicleType, 20);

        cout << "Vehicle Number: ";
        cin.getline(vehicleNumber, 15);

        cout << "Service Type (Engine/Oil/Brake): ";
        cin.getline(serviceType, 30);

        cout << "Service Cost: ";
        cin >> serviceCost;

        cout << "Spare Parts Cost: ";
        cin >> spareCost;

        totalCost = serviceCost + spareCost;
    }

    void display() const {
        cout << setw(6) << jobID
             << setw(15) << ownerName
             << setw(12) << vehicleType
             << setw(15) << vehicleNumber
             << setw(15) << serviceType
             << setw(10) << serviceCost
             << setw(10) << spareCost
             << setw(10) << totalCost << endl;
    }

    int getID() const {
        return jobID;
    }
};

// Function declarations
void addService();
void showAll();
void searchService();

// MAIN FUNCTION
int main() {
    int choice;

    do {
        cout << "\n\n===== AUTOMOBILE SERVICE MANAGEMENT =====";
        cout << "\n1. Add Vehicle for Service";
        cout << "\n2. Display All Services";
        cout << "\n3. Search by Job ID";
        cout << "\n4. Exit";
        cout << "\nEnter Choice: ";
        cin >> choice;

        switch (choice) {
            case 1: addService(); break;
            case 2: showAll(); break;
            case 3: searchService(); break;
            case 4: cout << "\nExiting System..."; break;
            default: cout << "\nInvalid Choice!";
        }
    } while (choice != 4);

    return 0;
}

// ADD SERVICE
void addService() {
    Vehicle v;
    ofstream file("service.dat", ios::binary | ios::app);

    v.input();
    file.write(reinterpret_cast<char*>(&v), sizeof(Vehicle));
    file.close();

    cout << "\nVehicle Service Record Added Successfully!";
}

// DISPLAY ALL
void showAll() {
    Vehicle v;
    ifstream file("service.dat", ios::binary);

    if (!file) {
        cout << "\nNo Service Records Found!";
        return;
    }

    cout << "\n"
         << setw(6) << "ID"
         << setw(15) << "Owner"
         << setw(12) << "Type"
         << setw(15) << "Number"
         << setw(15) << "Service"
         << setw(10) << "Serv₹"
         << setw(10) << "Spare₹"
         << setw(10) << "Total₹" << endl;

    while (file.read(reinterpret_cast<char*>(&v), sizeof(Vehicle))) {
        v.display();
    }
    file.close();
}

// SEARCH
void searchService() {
    Vehicle v;
    int id;
    bool found = false;

    cout << "\nEnter Job ID to Search: ";
    cin >> id;

    ifstream file("service.dat", ios::binary);

    while (file.read(reinterpret_cast<char*>(&v), sizeof(Vehicle))) {
        if (v.getID() == id) {
            cout << "\nRecord Found:\n";
            v.display();
            found = true;
            break;
        }
    }
    file.close();

    if (!found)
        cout << "\nRecord Not Found!";
}
#outputs🫂
https://raw.githubusercontent.com/braj-kishor/automobile-service-project/main/images/compile_result.jpg
