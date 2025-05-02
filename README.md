# cpp-final-project

#include <iostream>
#include <string>
#include <fstream>
#include <algorithm>
using namespace std;

void showIntro();
void showHelp();
void exploreRoom();
void fightMonster();
void pickUpItem(string item);
void gameLoop();
bool confirmExit(int attempts);
void saveLog(const string& log);
string toLower(const string& str);

string inventory[10];
int itemCount = 0;

int main() {
    showIntro();
    gameLoop();
    return 0;
}


void showIntro() {
    cout << " Welcome to the Text Adventure Game!" << endl;
    cout << "Type 'help' to see available commands." << endl;
}

void showHelp() {
    cout << " Commands:\n";
    cout << " - explore : Look around the current area\n";
    cout << " - fight   : Fight a monster (risky!)\n";
    cout << " - exit    : Exit the game\n";
    cout << " - help    : Show this help message\n";
}

void pickUpItem(string item) {
    if (itemCount < 10) {
        inventory[itemCount++] = item;
        cout << "You picked up: " << item << endl;
        saveLog("Picked up: " + item);
    } else {
        cout << "Your inventory is full!" << endl;
    }
}

void exploreRoom() {
    cout << "You explore the dark room and find a shiny sword." << endl;
    pickUpItem("Shiny Sword");
}

void fightMonster() {
    cout << "A wild goblin appears!" << endl;
    cout << "You swing your weapon...\n";
    if (itemCount > 0) {
        cout << "You slay the goblin using your " << inventory[itemCount - 1] << "!\n";
        saveLog("Fought and defeated a goblin.");
    } else {
        cout << "You have no weapon! The goblin defeats you.";
        saveLog("Lost a fight to a goblin.");
    }
}

void gameLoop() {
    string input;
    while (true) {
        cout << "\n> ";
        getline(cin, input);

        string command = toLower(input);

        if (command == "explore") {
            exploreRoom();
        } else if (command == "fight") {
            fightMonster();
        } else if (command == "help") {
            showHelp();
        } else if (command == "exit") {
            if (confirmExit(2)) {
                cout << "Thanks for playing!\n";
                break;
            } else {
                continue;
            }
        } else {
            cout << "Unknown command. Type 'help' for options.\n";
        }
    }
}

bool confirmExit(int attempts) {
    if (attempts <= 0) return true;

    string answer;
    cout << "Are you sure you want to exit? (yes/no): ";
    getline(cin, answer);

    transform(answer.begin(), answer.end(), answer.begin(), ::tolower);
    if (answer == "yes") return true;
    if (answer == "no") return false;

    cout << "Invalid input. Try again.\n";
    return confirmExit(attempts - 1);
}

void saveLog(const string& log) {
    ofstream logFile("game_log.txt", ios::app);
    if (logFile.is_open()) {
        logFile << log << endl;
        logFile.close();
    }
}

string toLower(const string& str) {
    string result = str;
    transform(result.begin(), result.end(), result.begin(), ::tolower);
    return result;
}
