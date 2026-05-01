# OOP-PROJECT
include <iostream>
#include <string>
using namespace std;
 
// ─── HELPER: print float to 2 decimals ───────────
void printFloat(float val) {
int whole = (int)val;
int decimal = (int)((val - whole) * 100 + 0.5);
cout << whole << ".";
if (decimal < 10) cout << "0";
cout << decimal;
}
 
// ─── HELPER: pad string with spaces ──────────────
void padStr(string text, int width) {
cout << text;
int spaces = width - (int)text.length();
for (int i = 0; i < spaces; i++) cout << " ";
}
 
// ─── HELPER: pad int with spaces ─────────────────
void padInt(int num, int width) {
int temp = num, digits = 1;
while (temp >= 10) { temp /= 10; digits++; }
cout << num;
for (int i = 0; i < width - digits; i++) cout << " ";
}
 
// ─── HELPER: print a line ────────────────────────
void line(char ch = '-', int len = 48) {
for (int i = 0; i < len; i++) cout << ch;
cout << endl;
}
 
 
// ══════════════════════════════════════════
// CLASS 1: MenuItem
// ══════════════════════════════════════════
class MenuItem {
public:
string name;
float price;
int quantity;
 
MenuItem() {
name = "";
price = 0;
quantity = 0;
}
 
void setItem(string n, float p, int q) {
name = n;
price = p;
quantity = q;
}
 
void displayItem(int index) {
cout << " " << index << ". ";
padStr(name, 20);
cout << "Rs. ";
printFloat(price);
cout << " Stock: " << quantity << endl;
}
};
 
 
// ══════════════════════════════════════════
// CLASS 2: Order
// ══════════════════════════════════════════
class Order {
public:
string customerName;
string itemNames[10];
int itemQty[10];
float itemPrice[10];
int totalItems;
 
Order() {
customerName = "";
totalItems = 0;
}
 
void setCustomer(string name) {
customerName = name;
}
 
void addItem(string name, int qty, float price) {
if (totalItems < 10) {
itemNames[totalItems] = name;
itemQty[totalItems] = qty;
itemPrice[totalItems] = price;
totalItems++;
cout << "\n [+] Added to order!\n";
} else {
cout << "\n [!] Order full (max 10 items).\n";
}
}
 
float calculateTotal() {
float total = 0;
for (int i = 0; i < totalItems; i++) {
total += itemQty[i] * itemPrice[i];
}
return total;
}
 
void printReceipt() {
cout << "\n";
line('=');
cout << " CAFETERIA RECEIPT\n";
line('=');
cout << " Customer: " << customerName << "\n";
line();
 
// header row
cout << " ";
padStr("Item", 16);
padStr("Qty", 6);
padStr("Price", 10);
cout << "Amount\n";
line();
 
for (int i = 0; i < totalItems; i++) {
float sub = itemQty[i] * itemPrice[i];
cout << " ";
padStr(itemNames[i], 16);
padInt(itemQty[i], 6);
cout << "Rs."; printFloat(itemPrice[i]); cout << " ";
cout << "Rs."; printFloat(sub);
cout << "\n";
}
 
float total = calculateTotal();
float tax = total * 0.05;
float net = total + tax;
 
line();
cout << " Subtotal : Rs."; printFloat(total); cout << "\n";
cout << " Service(5%): Rs."; printFloat(tax); cout << "\n";
line('=');
cout << " NET TOTAL : Rs."; printFloat(net); cout << "\n";
line('=');
cout << " * Enjoy your meal! *\n";
line('=');
}
};
 
 
// ══════════════════════════════════════════
// CLASS 3: Cafeteria
// ══════════════════════════════════════════
class Cafeteria {
public:
MenuItem menu[10];
int menuCount;
 
Cafeteria() {
menuCount = 0;
loadMenu();
}
 
void loadMenu() {
menu[0].setItem("Biryani", 150.0, 20);
menu[1].setItem("Burger", 80.0, 30);
menu[2].setItem("Sandwich", 60.0, 25);
menu[3].setItem("Karahi", 180.0, 15);
menu[4].setItem("Cold Drink", 40.0, 50);
menu[5].setItem("Tea / Chai", 20.0, 40);
menu[6].setItem("Samosa (2pcs)", 30.0, 35);
menu[7].setItem("Fruit Juice", 60.0, 20);
menuCount = 8;
}
 
void showMenu() {
cout << "\n";
line();
cout << " CAFETERIA MENU\n";
line();
for (int i = 0; i < menuCount; i++) {
menu[i].displayItem(i + 1);
}
line();
}
 
void showMainMenu() {
cout << "\n";
line();
cout << " CAFETERIA MANAGEMENT SYSTEM\n";
line();
cout << " 1. View Menu\n";
cout << " 2. Place Order\n";
cout << " 3. About\n";
cout << " 0. Exit\n";
line();
}
 
void placeOrder() {
Order order;
 
string name;
cout << "\n Enter your name: ";
cin.ignore();
getline(cin, name);
order.setCustomer(name);
 
int choice;
do {
showMenu();
cout << " Select item (1-" << menuCount << ") or 0 to finish: ";
cin >> choice;
 
if (choice >= 1 && choice <= menuCount) {
int idx = choice - 1;
 
if (menu[idx].quantity == 0) {
cout << "\n [!] Sorry, out of stock!\n";
continue;
}
 
int qty;
cout << " How many? ";
cin >> qty;
 
if (qty > menu[idx].quantity) {
cout << "\n [!] Only " << menu[idx].quantity << " available.\n";
continue;
}
 
order.addItem(menu[idx].name, qty, menu[idx].price);
menu[idx].quantity -= qty;
 
} else if (choice != 0) {
cout << "\n [!] Invalid choice.\n";
}
 
} while (choice != 0);
 
if (order.totalItems > 0) {
order.printReceipt();
} else {
cout << "\n [!] No items ordered.\n";
}
}
 
void showAbout() {
cout << "\n";
line('=');
cout << " Project : Cafeteria Management System\n";
cout << " Subject : OOP in C++\n";
cout << " IDE : Dev-C++ / Code::Blocks\n";
cout << "\n OOP CONCEPTS:\n";
cout << " - Class (MenuItem, Order, Cafeteria)\n";
cout << " - Object (menu[], order, caf)\n";
cout << " - Constructor (auto-load menu)\n";
cout << " - Methods (setItem, addItem, etc.)\n";
line('=');
}
 
void run() {
int choice;
do {
showMainMenu();
cout << " Enter choice: ";
cin >> choice;
 
switch (choice) {
case 1: showMenu(); break;
case 2: placeOrder(); break;
case 3: showAbout(); break;
case 0: cout << "\n Goodbye!\n"; break;
default: cout << "\n [!] Invalid.\n";
}
 
} while (choice != 0);
}
};
 
 
// ══════════════════════════════════════════
// MAIN
// ══════════════════════════════════════════
int main() {
Cafeteria caf;
caf.run();
return 0;
}
