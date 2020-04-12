# Variables 
    
    #include <iostream>

    using namespace std;

    int main()
    {
        int female = 4;
        int male = 5;
        int humans = 0;
        int cat = 1;
        int dog = 1;
        int animals = 0;
        int livingBeings = 0;

        animals = cat + cat + cat + dog + dog + dog;

        humans = female + male;

        livingBeings = humans + animals;

        cout << "In this room are " << humans << " humans and " << animals << " animals." <<endl;

        cout << "Living beings all together " << livingBeings << "." << endl;
        
        return 0;
    }
