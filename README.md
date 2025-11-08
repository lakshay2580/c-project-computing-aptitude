#include <iostream> 
#include <fstream> 
#include <iomanip> 
#include <string> 
using namespace std; 
struct Patient { 
int id; 
string name; 
int age; 
string gender; 
string disease; 
}; 
// Function prototypes 
void addPatient(); 
void displayAllPatients(); 
void searchPatient(); 
void updatePatient(); 
int main() { 
int choice; 
do { 
cout << "\n--- Hospital Management System ---\n"; 
cout << "1. Add Patient\n"; 
cout << "2. Display All Patients\n"; 
cout << "3. Search Patient by ID\n"; 
cout << "4. Update Patient Record\n"; 
cout << "5. Exit\n"; 
cout << "Enter your choice: "; 
cin >> choice; 
cin.ignore(); 
switch (choice) { 
case 1: addPatient(); break; 
case 2: displayAllPatients(); break; 
case 3: searchPatient(); break; 
case 4: updatePatient(); break; 
case 5: cout << "Exiting...\n"; break; 
default: cout << "Invalid choice! Try again.\n"; 
} 
} while (choice != 5); 
return 0; 
} 
// Add patient 
void addPatient() { 
Patient p; 
ofstream file("patients.txt", ios::app); 
cout << "\nEnter Patient ID: "; 
cin >> p.id; 
cin.ignore(); 
cout << "Enter Name: "; 
getline(cin, p.name); 
cout << "Enter Age: "; 
cin >> p.age; 
cin.ignore(); 
cout << "Enter Gender: "; 
getline(cin, p.gender); 
cout << "Enter Disease: "; 
getline(cin, p.disease); 
f
 ile << p.id << "," << p.name << "," << p.age << "," << p.gender << "," << 
p.disease << endl; 
f
 ile.close(); 
cout << "\n✅ Patient record added successfully!\n"; 
} 
// Display all patients 
void displayAllPatients() { 
ifstream file("patients.txt"); 
if (!file) { 
cout << "\n❌ No patient records found.\n"; 
return; 
} 
cout << "\n--- Patient Records ---\n"; 
cout << left << setw(10) << "ID" 
<< setw(20) << "Name" 
<< setw(10) << "Age" 
<< setw(10) << "Gender" 
<< setw(20) << "Disease" << endl; 
cout << string(70, '-') << endl; 
string line; 
bool found = false; 
while (getline(file, line)) { 
if (line.empty()) continue; 
size_t pos = 0; 
string tokens[5]; 
int i = 0; 
while ((pos = line.find(',')) != string::npos && i < 4) { 
tokens[i++] = line.substr(0, pos); 
line.erase(0, pos + 1); 
} 
tokens[i] = line; 
cout << left << setw(10) << tokens[0] 
<< setw(20) << tokens[1] 
<< setw(10) << tokens[2] 
<< setw(10) << tokens[3] 
<< setw(20) << tokens[4] << endl; 
found = true; 
} 
if (!found) 
cout << "\nNo patient data available.\n"; 
f
 ile.close(); 
} 
// Search patient by ID 
void searchPatient() { 
int id; 
cout << "\nEnter Patient ID to search: "; 
cin >> id; 
ifstream file("patients.txt"); 
if (!file) { 
cout << "\n❌ No patient records found.\n"; 
return; 
} 
string line; 
bool found = false; 
while (getline(file, line)) { 
if (line.empty()) continue; 
size_t pos = line.find(','); 
int storedID = stoi(line.substr(0, pos)); 
if (storedID == id) { 
cout << "\n✅ Patient Record Found:\n"; 
cout << "--------------------------------------\n"; 
size_t start = 0, end; 
string fields[5]; 
int i = 0; 
while ((end = line.find(',', start)) != string::npos && i < 4) { 
f
 ields[i++] = line.substr(start, end - start); 
start = end + 1; 
} 
f
 ields[i] = line.substr(start); 
cout << "ID: " << fields[0] << endl; 
cout << "Name: " << fields[1] << endl; 
cout << "Age: " << fields[2] << endl; 
cout << "Gender: " << fields[3] << endl; 
cout << "Disease: " << fields[4] << endl; 
found = true; 
break; 
} 
} 
if (!found) 
cout << "\n❌ Patient with ID " << id << " not found.\n"; 
f
 ile.close(); 
} 
// Update patient record 
void updatePatient() { 
int id; 
cout << "\nEnter Patient ID to update: "; 
cin >> id; 
cin.ignore(); 
ifstream inFile("patients.txt"); 
if (!inFile) { 
cout << "\n❌ No patient records found.\n"; 
return; 
} 
ofstream tempFile("temp.txt"); 
string line; 
bool updated = false; 
while (getline(inFile, line)) { 
if (line.empty()) continue; 
size_t pos = line.find(','); 
int storedID = stoi(line.substr(0, pos)); 
if (storedID == id) { 
Patient p; 
cout << "\nEnter new details for Patient ID " << id << ":\n"; 
cout << "Name: "; 
getline(cin, p.name); 
cout << "Age: "; 
cin >> p.age; 
cin.ignore(); 
cout << "Gender: "; 
getline(cin, p.gender); 
cout << "Disease: "; 
getline(cin, p.disease); 
p.id = id; 
tempFile << p.id << "," << p.name << "," << p.age << "," << p.gender 
<< "," << p.disease << endl; 
updated = true; 
} else { 
tempFile << line << endl; 
} 
} 
inFile.close(); 
tempFile.close(); 
remove("patients.txt"); 
rename("temp.txt", "patients.txt"); 
if (updated) 
cout << "\n✅ Record updated successfully!\n"; 
else 
cout << "\n❌ Patient ID not found.\n"; 
}
