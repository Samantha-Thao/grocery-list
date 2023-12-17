#include <fstream>
#include <iomanip>
#include <iostream>
#include <map>

using namespace std;

// Class with public and private sections
class ItemFinder
{
private:
	
	// Map to contain list of item and frequency
	map<string, int> itemFrequency;


public:

	// Calls loadData() and writeDataToFile() to take clutter out of main()
	void startup(const string& inputFilename, const string& outputFilename)
	{
		loadData(inputFilename);
		writeDataToFile(outputFilename);
	}

	// Writes item and frequency from map to frequency.dat file
	void writeDataToFile(const string& outputFilename)
	{
		ofstream outputFile(outputFilename);

		// Loops through map and writes item with frequency to file
		for (const auto& pair : itemFrequency)
		{
			outputFile << pair.first << ": " << pair.second << endl;
		}

		outputFile.close();
	}

	// Read text file and add to map.
	void loadData(const string& inputFilename)
	{
		ifstream inputFile(inputFilename);

		// Check if our input file was opened successfully. 
		if (!inputFile.is_open())
		{
			cerr << "Could not open input file." << '\n';
			exit(EXIT_FAILURE);
		}

		// Iterate through input file, storing them into our map. 
		string item;

		while (inputFile >> item)
		{
			itemFrequency[item]++;
		}
	}



	// Menu Option #1.
	void SearchItem(const string& item)
	{
		cout << item << " Appears " << itemFrequency[item] << " times." << endl;
	}



	// Menu Option #2.
	void PrintItemsWithNumbers()
	{
		for (const auto& pair : itemFrequency)
		{
			cout << setw(15) << left << pair.first << " " << pair.second << endl;
		}
	}



	// Menu Option #3.
	void PrintHistogram()
	{
		for (const auto& pair : itemFrequency)
		{
			cout << setw(15) << left << pair.first << " ";

			for (int i = 0; i < pair.second; i++)
			{
				cout << "*";
			}
			cout << endl;
		}
	}
};



// Diplay Main Menu and switch for navigating options with input validation
void mainMenu(ItemFinder& itemFinder)
{
	int userInput{ 0 };

	while (userInput != 4)
	{
		cout << "Main Menu" << '\n';
		cout << "Menu Option 1: Item Search" << '\n';
		cout << "Menu Option 2: Item Print with Numbers" << '\n';
		cout << "Menu Option 3: Item Print with Histogram" << '\n';
		cout << "Menu Option 4: Exit Program" << '\n';

		cout << "Select Menu Option 1-4: " << '\n';
		cin >> userInput;

		string userItem{ "" };

		switch (userInput)
		{
		case 1:
			cout << "Please Enter an Item: " << '\n';
			cin >> userItem;
			itemFinder.SearchItem(userItem);
			cout << '\n';
			break;

		case 2:
			itemFinder.PrintItemsWithNumbers();
			cout << '\n';
			break;

		case 3:
			itemFinder.PrintHistogram();
			cout << '\n';
			break;

		case 4:
			cout << "Exiting Program. Saving Progress to frequency.dat" << '\n';
			break;

		default:
			cout << "Invalid Entry. Please choose a number 1-4." << '\n';
		}
	}
}

// Main
int main()
{
	// Instantiate an ItemFinder object. 
	ItemFinder itemFinder;

	itemFinder.startup("CS210_Project_Three_Input_File.txt", "frequency.dat");

	// Call mainMenu() function
	mainMenu(itemFinder);

	return 0;
}
