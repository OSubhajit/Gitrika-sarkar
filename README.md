# Gitrika-sarkar
<br>
Gitrika sarkar is a simple chatbot.

#include <iostream>
#include <string>
#include <limits>
#include <map>
#include <sstream>
#include <functional>
#include <vector>
#include <cmath>

// Define a struct to hold conversation context
struct ConversationContext {
    std::string lastUserInput;
    bool isGreetingReceived;
};

// Function to perform sentiment analysis
int analyzeSentiment(const std::string& input) {
    std::map<std::string, int> sentimentLexicon = {
        {"happy", 1},
        {"angry", -1},
        {"sad", -1},
    };

    int sentimentScore = 0;
    for (const auto& pair : sentimentLexicon) {
        if (input.find(pair.first) != std::string::npos) {
            sentimentScore += pair.second;
        }
    }
    return sentimentScore;
}

// Function to evaluate a mathematical expression
double evaluateExpression(const std::string& expression) {
    try {
        size_t pos;
        double result = std::stod(expression, &pos);
        if (pos == expression.length()) {
            return result;
        } else {
            throw std::invalid_argument("Invalid expression");
        }
    } catch (const std::invalid_argument&) {
        throw;
    }
}

// Function to calculate area of shapes
double calculateArea(const std::string& shape, const std::vector<double>& params) {
    try {
        if (shape == "square" && params.size() == 1) {
            double side = params[0];
            return side * side;
        } else if (shape == "rectangle" && params.size() == 2) {
            double length = params[0];
            double width = params[1];
            return length * width;
        } else if (shape == "circle" && params.size() == 1) {
            double radius = params[0];
            return M_PI * radius * radius;
        } else {
            throw std::invalid_argument("Invalid shape or parameters");
        }
    } catch (const std::invalid_argument& e) {
        throw;
    }
}

// Function to calculate square root
double calculateSquareRoot(double number) {
    if (number < 0) {
        throw std::invalid_argument("Cannot calculate square root of a negative number");
    }
    return sqrt(number);
}

// Function to solve quantity equation of the form ax + b = c
double solveQuantityEquation(double a, double b, double c) {
    if (a == 0) {
        throw std::invalid_argument("Coefficient 'a' cannot be zero");
    }
    return (c - b) / a;
}

// Function to handle trigonometric functions
double calculateTrigonometricFunction(const std::string& function, double angle) {
    if (function == "sin") {
        return sin(angle);
    } else if (function == "cos") {
        return cos(angle);
    } else if (function == "tan") {
        return tan(angle);
    } else {
        throw std::invalid_argument("Invalid trigonometric function");
    }
}

// Function to calculate probability of an event
double calculateProbability(double favorableOutcomes, double totalOutcomes) {
    if (totalOutcomes <= 0) {
        throw std::invalid_argument("Total outcomes must be greater than zero");
    }
    return favorableOutcomes / totalOutcomes;
}

// Function to get response based on user input
std::string getResponse(const std::string& input, int sentimentScore, ConversationContext& context) {
    std::map<std::string, std::function<std::string()>> dynamicResponses = {
        {"how are you", []() {
            return "I'm doing well, thank you!";
        }},
        {"what is your name", []() {
            return "My name is Gitrika.";
        }},
        {"may I know your name plz", []() { // New response added here
            return "My name is Gitrika.";
        }},
        {"who are you", []() {
            return "A personal assistant of Mr. Subhajit Sarkar.";
        }},
        {"tell me a joke", []() {
            std::vector<std::string> jokes = {
                "Why don't scientists trust atoms? Because they make up everything!",
            };
            return jokes[rand() % jokes.size()];
        }},
        {"what time is it", []() {
            return "I don't have access to the current time.";
        }},
        {"area", []() {
            return "I can help you calculate the area of shapes like squares, rectangles, and circles. What shape do you need the area for?";
        }},
        {"perimeter", []() {
            return "I can help you calculate the perimeter of shapes like squares, rectangles, and circles. What shape do you need the perimeter for?";
        }},
        // Add more mathematical queries here
    };
    for (const auto& pair : dynamicResponses) {
        if (input.find(pair.first) != std::string::npos) {
            return pair.second();
        }
    }
   if (input.find("BODMAS") != std::string::npos) {
        return "BODMAS stands for Brackets, Orders (i.e. powers and square roots, etc.), Division and Multiplication, and Addition and Subtraction. It's an order of operations used in mathematics to ensure that calculations are performed in the correct order.";
    }
  if (input.find("root over") != std::string::npos) {
        std::istringstream iss(input);
        std::string dummy, rootType;
        double number;
        iss >> dummy >> dummy >> rootType >> number;
        if (rootType == "square") {
            try {
                double result = calculateSquareRoot(number);
                return "The square root of " + std::to_string(number) + " is: " + std::to_string(result);
            } catch (const std::invalid_argument& e) {
                return e.what();
            }
        } else {
            return "Unsupported root type";
        }
    }
    if (input.find("quantity equation") != std::string::npos) {
        std::istringstream iss(input);
        std::string dummy, equation;
        iss >> dummy >> dummy >> equation;
        double a, b, c;
        char x;
        std::istringstream equationStream(equation);
        equationStream >> a >> x >> dummy >> b >> dummy >> c;
        try {
            double result = solveQuantityEquation(a, b, c);
            return "The solution for the equation is: " + std::to_string(result);
        } catch (const std::invalid_argument& e) {
            return e.what();
        }
    }
  if (input.find("trigonometry") != std::string::npos) {
        std::istringstream iss(input);
        std::string trigFunction;
        char dummy;
        double angle;
        iss >> trigFunction >> dummy >> angle;
        try {
            double result = calculateTrigonometricFunction(trigFunction, angle);
            return "The " + trigFunction + " of " + std::to_string(angle) + " degrees is: " + std::to_string(result);
        } catch (const std::invalid_argument
& e) {
            return e.what();
        }
    }
   if (input.find("probability") != std::string::npos) {
        // Example: "probability of rolling a six on a fair six-sided die"
        // Implement probability calculation logic here
        // For now, let's return a placeholder response
        return "Probability calculation not implemented yet.";
    }
   // Default response for unrecognized queries
    if (sentimentScore > 0) {
        return "It sounds like you're feeling positive!";
    } else if (sentimentScore < 0) {
        return "I'm sorry to hear that you're feeling negative. Is there anything I can do to help?";
    }

   try {
        double result = evaluateExpression(input);
        return "The result is: " + std::to_string(result);
    } catch (const std::invalid_argument&) {
        return "I'm not sure how to respond to that.";
    }
}
// Function to safely read passwords with masking
std::string readPassword() {
    std::string password;
    char ch;
    std::cout << "Enter your password: ";
    ch = std::getchar();
    while (ch != '\n') {
        if (ch == '\b') { // Handle backspace
            if (!password.empty()) {
                std::cout << "\b \b";
                password.pop_back();
            }
        } else {
            password.push_back(ch);
            std::cout << '*';
        }
        ch = std::getchar();
    }
    return password;
}

// Function to print welcome banner
void printWelcomeBanner(const std::string& username) {
    std::cout << "\n\nWelcome back Mr. " << username << "\n";
    std::cout << "---------------------------------\n";
}

int main() {
    const std::string yourUsername = "subhajit";
    const std::string yourPassword = "classmate!@#$";
    std::string username, password;

 std::cout << "Enter your username: ";
    std::cin >> username;
    std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n'); // Clear the buffer
    password = readPassword();

   if (username != yourUsername || password != yourPassword) {
        std::cout << "\nUnauthorized access. Please enter the correct credentials.\n";
        return 1;
    }

   printWelcomeBanner(username);

  std::string userInput;
    ConversationContext context;
    context.isGreetingReceived = false;
    while (true) {
        std::cout << "> ";
        std::getline(std::cin, userInput);
        if (userInput == "bye") {
            std::cout << "Goodbye!\n";
            break;
        }
        int sentimentScore =analyzeSentiment(userInput);
        std::cout << getResponse(userInput, sentimentScore, context) << std::endl;
    if (userInput.find("hi") != std::string::npos) {
            context.isGreetingReceived = true;
        }
        context.lastUserInput = userInput;
    }
   return 0;
}
