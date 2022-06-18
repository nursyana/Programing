OPERATING SYSTEM - PROGRAMMING GROUP PROJECT

Group Members:
1. Alfin Najeehah binti Zahid - 2019618
2. Nur Syazana binti Abdul Kadir - 2016730
3.

/* A Program to Implement the Banker's Algorithm */

#include <iostream>
using namespace std;

// Number of processes
const int process = 5;
// Number of resources 	
const int resource = 3;

int available[resource];// Current available resources
int maximum[process][resource];// Maximum resources required by the process
int allocation[process][resource];// Resources allocated to the process
int need[process][resource];// Resources needed for each process
int safeSeq[process];// Safe sequence

// Compare X<Y
bool isLess(int a[], int b[], int len){
    bool less = true;
    for(int i = 0; i < len; i++){
        if(a[i] > b[i]){
            less = false;
            break;
        }
    }
 
    return less;
}

// Function to find the system is in a safe state or not
bool isSafe(){
	int work[resource];
    bool finish[process];
    bool safe = true;
 
    for(int i = 0; i < resource; i++){
        work[i] = available[i];
    }
 
    for(int i = 0; i < process; i++){
        finish[i] = false;
    }
 
    int finishCount = 0;
    int seqIndex = 0;
    while(finishCount < process){
        bool flag = false;
 
        for(int i = 0; i < process; i++){
            if(finish[i] == false && isLess(need[i], work, resource)){
                finish[i] = true;
                finishCount++;
                for(int j = 0; j < resource; j++){
                    work[j] += allocation[i][j];
                }
 
                safeSeq[seqIndex++] = i;
                flag = true;
            }
        }
        
        // Could not find the next process in the safe sequence
		if (flag == false)
		{
			cout << "\n-1\n";
			return false;
		}
	}
	
	// If the system is in a safe state
	cout << "\n0\n" << endl;;
	for (int i = 0; i < process ; i++)
		cout << "Thread " << safeSeq[i] << " ";
		cout << endl;
}

int bank(){
    int pi;
    int request[resource];
    cout << "\nEnter process number and its request:\n";
    cin >> pi;
    for(int i = 0; i < resource; i++){
        cin >> request[i];
    }
    
    if(!isLess(request, need[pi], resource)){
        cout << "Error!\n";
        return 0;
    }
 
    if(!isLess(request, available, resource)){
        cout << "Wait for available resource.\n";
        return 0;
    }
 
    for(int i = 0; i < resource; i++)
	{
        available[i] -= request[i];
        allocation[pi][i] += request[i];
        need[pi][i] -= request[i];
    }
    
	if(isSafe()){
    }
    
    else{
        for(int i = 0; i < resource; i++){
            available[i] += request[i];
            allocation[pi][i] -= request[i];
            need[pi][i] += request[i];
        }
    }

}
    
int main(){
    cout << "Enter the available resources:\n";
    for(int i = 0; i < resource; i++){
        cin >> available[i];
    }
 
    cout << "\nEnter the maximum matrix:\n";
    for(int i = 0; i < process; i++){
        for(int j = 0; j < resource; j++){
            cin >> maximum[i][j];
        }
    }
 
    cout << "\nEnter the allocation matrix:\n";
    for(int i = 0; i < process; i++){
        for(int j = 0; j < resource; j++){
            cin >> allocation[i][j];
        }
    }
 
    for(int i = 0; i < process; i++){
        for(int j = 0; j < resource; j++){
            need[i][j] = maximum[i][j] - allocation[i][j];
            available[j] -= allocation[i][j];
        }
    }
    
    // Check whether the system is in a safe state or not
	isSafe();
	
	// Make a request
	int ch;
	cout << "\nDo you have any additional request? (Yes = 1 | No = 0): ";
	cin >> ch;
	if (ch == 1)
	{
		bank();
	}
	else
	{
		return 0;
	}
	
    return 0;
}
