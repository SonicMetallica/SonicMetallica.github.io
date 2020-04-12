# Userinput
    
    #include <iostream>

    using namespace std;

    int main()
    {
        string input;

        cout << "Enter your name " << flush;
        cin >> input;

        cout << "You did enter " << input << ".";

        string gender;

        cout << "Enter your gender " << flush;
        cin >> gender;

        cout << "You did enter " << gender << ".";

        string age;
        
        cout << "Enter your age " << flush;
        cin >> age;

        cout << "You did enter " << age << ".";
    }
