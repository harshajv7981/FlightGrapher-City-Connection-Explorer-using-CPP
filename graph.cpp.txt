#include <iostream>
#include <fstream>
#include <string>
#include <vector>
#include <unordered_map>

using namespace std;

// Define a struct to store information about each city
struct City {
    string name;
    vector<string> destinations;
};

// Define a graph data structure to store information about all cities and their destinations
class Graph {
public:
    // Constructor
    Graph() {}

    // Add a new city to the graph
    void addCity(string cityName) {
        City newCity;
        newCity.name = cityName;
        cities[cityName] = newCity;
    }

    // Add a new destination for an existing city
    void addDestination(string cityName, string destinationName) {
        cities[cityName].destinations.push_back(destinationName);
    }

    // Get all destinations for a given city
    vector<string> getDestinations(string cityName) {
        return cities[cityName].destinations;
    }

private:
    unordered_map<string, City> cities;
};

// Read in data from city.name and flight.txt files and store it in a graph data structure
Graph readData() {
    Graph graph;

    // Read in data from city.name file
    ifstream cityFile("city.name");
    if (cityFile.is_open()) {
        string cityName;
        while (getline(cityFile, cityName)) {
            graph.addCity(cityName);
        }
        cityFile.close();
    } else {
        cout << "Unable to open file";
        exit(1);
    }

    // Read in data from flight.txt file
    ifstream flightFile("flight.txt");
    if (flightFile.is_open()) {
        string line;
        while (getline(flightFile, line)) {
            string sourceCity = line.substr(0, line.find(" "));
            string destinationCity = line.substr(line.find(" ") + 1);
            graph.addDestination(sourceCity, destinationCity);
        }
        flightFile.close();
    } else {
        cout << "Unable to open file";
        exit(1);
    }

    return graph;
}
