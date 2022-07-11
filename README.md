# Lab2

//dollar.cpp


#ifndef DOLLAR_H
#define DOLLAR_H

#include <iostream>
#include "currency.cpp"
using namespace std;

// derived class Dollar from Currency class
class Dollar : public Currency
{

    private:
        string currencyName;
    
    public:
        Dollar()
        {
            currencyName = "dollar";
        }
    
        Dollar(double value) : Currency(value)
        {
            currencyName = "Dollar";
        }
    
        void toString()
        {
            Currency::toString();
            cout << " " << currencyName << " ";
        }
};
#endif


==================

//pound.cpp


#ifndef POUND_H
#define POUND_H

#include <iostream>
#include "currency.cpp"
using namespace std;

// // derived class Pound from Currency class
class Pound : public Currency
{
    private:
        string currencyName;
    
    public:
        //constructor
        Pound(double value) : Currency(value)
        {
            currencyName = "Pound";
        }
    
        void toString()
        {
            Currency::toString();
            cout << " " << currencyName << " ";
        }
};
#endif


======================

currency.cpp
#ifndef CURRENCY_H
#define CURRENCY_H

#include <iostream>
#include <iomanip>
#include <cmath>
using namespace std;

// abstract base class called Currency
class Currency
{
    private:
        int whole;
        int fraction;
    
    public:
        // default constructor
        Currency()
        {
            whole = 0;
            fraction = 0;
        }
        // parametered constructor
        Currency(double input)
        {
            if (input < 0)
                throw "Invalid value";
            whole = int(input);
            fraction = (int)round(fabs(input - trunc(input)) * 1e2);
        }
        Currency(const Currency &p1)
        {
            whole = p1.whole;
            fraction = p1.fraction;
        }
        // destructor
        ~Currency()
        {
        }
        // getters
        int getWhole()
        {
            return whole;
        }
    
        int getFraction()
        {
            return fraction;
        }
        
        // setters
        void setWhole(int whole)
        {
            this->whole = whole;
        }
        void setFraction(int fraction)
        {
            this->fraction = fraction;
        }
        
        // adding an input object of the same currency
        void add(double input)
        {
            whole = whole + int(input);
            fraction = fraction + (int)round(fabs(input - trunc(input)) * 1e2);
        }
    
        // subtracting an input object of the same currency
        void subtract(double input)
        {
            int inputwhole = int(input);
            int inputfraction = (int)round(fabs(input - trunc(input)) * 1e2);
            if (whole - inputwhole < 0 || fraction - inputfraction < 0)
            {
                cout << "Invalid subtraction" << endl;
            }
            else
            {
                whole = whole - inputwhole;
                fraction = fraction - inputfraction;
            }
        }
        //  comparing an input object of the same currency for equality/inequality.
        bool isEqual(Currency c1, Currency c2)
        {
            return (c1.whole == c2.whole &&  c1.fraction == c2.fraction);
        }
        //  comparing an input object of the same currency to identify which object is larger or smaller.
        bool isGreater(Currency c1, Currency c2)
        {
            if (c1.whole > c2.whole)
                return true;
            if (c1.fraction == c2.fraction)
            {
                if (c1.fraction > c2.fraction)
                    return true;
                else
                    return false;
            }
            return false;
        }
        // print the name and value of a currency object in the form "xx.yy"
        virtual void toString()
        {
            cout << fixed << setprecision(2);
            cout << getWhole() << '.' << setw(2) << setfill('0') << getFraction();
        }
};
#endif

=====================

//main.cpp


#include <iostream>
#include "currency.cpp"
#include "dollar.cpp"
#include "pound.cpp"
using namespace std;

int main()
{
    // declare variables 
    char operationName, currencyChar;
    string currencyName;
    double value;
    // declare an array of 2 Currency pointers
    Currency* currencyPointer[2];
    // Set the first reference in the array to a Pound object
    currencyPointer[0] = new Pound(0.00);
    // Set the second reference to a Dollar object
    currencyPointer[1] = new Dollar(0.00);
    // toString the initial values
    currencyPointer[0]->toString();
    currencyPointer[1]->toString();
    while (true)
    {
        cout << endl;
        // get user input
        cin >> operationName >> currencyChar >> value >> currencyName;
        // check the operation name
        if (operationName == 'a')
        {
            // addition
            if (currencyChar == 'd' && currencyName == "dollar")
                currencyPointer[1]->add(value);
            else if (currencyChar == 'p' && currencyName == "pound")
                currencyPointer[0]->add(value);
            else
                cout << "Invalid addition" << endl;
            currencyPointer[0]->toString();
            currencyPointer[1]->toString();
        } // subtraction
        else if (operationName == 's')
        {
            if (currencyChar == 'd' && currencyName == "dollar")
                currencyPointer[1]->subtract(value);
            else if (currencyChar == 'p' && currencyName == "pound")
                currencyPointer[0]->subtract(value);
            else
                cout << "Invalid subtraction" << endl;
            currencyPointer[0]->toString();
            currencyPointer[1]->toString();
        } // not valid operationName end the program
        else
            break;
    }
    return 0;
}


