
#include <iostream>
#include<fstream>
using namespace std;

class FloatArray
{
protected:
    int Size;
    float *arr;

public:
    int c=0;/// c is a counter.
    FloatArray() {}
    FloatArray(int s)
    {
        Size=s;
        arr=new float[Size];/// We allocate a dynamic array.

    }

    /** This function add an element by the normal method.
    for example if we add elements 22,1.5,0 the array will be {22,1.5,0}
    **/
    virtual  bool ADD(float num)
    {
        if(c>=Size)
        {
            return false;
        }
        else
        {
            arr[c]=num;
            c++;
        }
    }
    friend ifstream&operator>>(ifstream&in,FloatArray *obj1)
    {
        for(int i=0; i<obj1->Size; i++)
        {
            float elem;
            in>>elem;
            obj1->ADD(elem);


        }

    }
    friend ofstream&operator<<(ofstream&out,FloatArray *obj2)
    {
        out<<obj2->c<<"|"<<"  ";
        for(int i=0; i<obj2->c; i++)

        {
            out<<obj2->arr[i]<<"  ";
        }
        out<<endl;
        return out;
    }

    ~FloatArray()
    {
        delete[]arr;
    }

};
class SortedArray:public FloatArray
{protected:
    int counter;
public:
    SortedArray() {}
    SortedArray(int s):FloatArray(s) {};

    /**This function take an array and sorted it for example if we have an array that contain
    {2} and we add an element 1 to the array,the array will be {1,2}.
    **/
    bool ADD(float num)
    {
        counter++;

        int index=c;

        for(int i=0; i<c; i++)
        {
            if(arr[i]>num)
            {

                index=i;///Here we save the position where we will insert the element.

                for(int j=c; j>=index; j--)
                {
                    arr[j]=arr[j-1];

                }
                break;

            }



        }
        arr[index]=num;
            c++;

    }

};
class FrontArray:public FloatArray
{
public:
    FrontArray() {}
    FrontArray(int s):FloatArray(s) {};

    /**This function take an array and put the last element in the front of the array
    for example if we have an array that contain {4}
    and we add an element 4 to the array,the array will be {8,4}.
    **/
    bool ADD(float num)
    {
        for(int i=c; i>0; i--)
        {
            arr[i]=arr[i-1];
        }
        arr[0]=num;
        c++;

    }
};
class PositiveArray:public SortedArray
{
public:
    PositiveArray () {}
    PositiveArray (int s):SortedArray(s) {};

    /**
    This function take an array and add just positive elements to it
    for example if we want to add -5,-1,10,8,-6 the array will be {8,10}
    because it inherit from class Sorted .
    **/

    bool ADD(float num)
    {
        if(num>0)
        {
            SortedArray::ADD(num);
        }

    }
};
class NegativeArray:public SortedArray
{
public:
    NegativeArray () {}
    NegativeArray (int s):SortedArray(s) {};

    /**
    This function take an array and add just Negative elements to it
    for example if we want to add -5,-1,10,8,-6 the array will be {-6,-5,-1}
    because it inherit from class Sorted .
    **/
    bool ADD(float num)
    {
        if(num<0)
        {
            SortedArray::ADD(num);
        }
    }
};
int main()
{
    string in_file,out_file;
    cout<<"Enter the name of the input file "<<endl;
    cin>>in_file;///We get the name of the input file from the user .
    ifstream f(in_file);///This file to input the data.

    cout<<"Enter the name of the output file "<<endl;
    cin>>out_file;///We get the name of the output file from the user .
    ofstream out(out_file);///We create file to output the data .

    FloatArray *ptr[100];///We create a pointer from class FloatArray.
    string s;
    int SIZE;
    int Counter=0;
  for(int Counter=0;f>>s>>SIZE;Counter++){///This loop will work until the file read name and size.

       /**We will create an object from class FloatArray
        and it will call the parameterized constructor of class FloatArray .
        **/
        if(s=="Array")
        {
            ptr[Counter]=new FloatArray(SIZE);
        }
        else if(s=="Sorted")
        {
            ptr[Counter]=new SortedArray(SIZE);

        }
        else if(s=="Positive")
        {
            ptr[Counter]=new PositiveArray(SIZE);
        }
        else if(s=="Negative")
        {
            ptr[Counter]=new NegativeArray(SIZE);
        }
        else if(s=="Front")
        {
            ptr[Counter]=new FrontArray(SIZE);

        }


        f>>ptr[Counter];///Here we will call the operator >>;
        out<<ptr[Counter];///Here we will call the operator <<;
    }
    for(int i=0;i<Counter;i++){///We delete ptr .
    delete  ptr[i];
}

    f.close();///We close the input file.
    out.close();///we close the output file.

    return 0;
}

