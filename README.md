#include <stdio.h>


typedef enum { //defining the 2 states locked and unlocked
    STATE_LOCKED,
    STATE_UNLOCKED
} State;
typedef enum { //defining the inputs, I chose QR to be similar to the metro in Armenia and actually its easier than having other type of inputs
    INPUT_QR_VALID,
    INPUT_QR_INVALID,
    INPUT_DELAY // I want the door to be locked after 10 seconds when its unlocked for the next passenger not to pass
} Input;

State transition(State currentState, Input input) {
    switch (currentState) {
        case STATE_LOCKED:
            if (input == INPUT_QR_VALID) //When the QR is valid
                return STATE_UNLOCKED;
            else // When the QR is Invalid
                return STATE_LOCKED;

        case STATE_UNLOCKED:
            if (input == INPUT_DELAY)
                return STATE_LOCKED;
            else
                return STATE_UNLOCKED; // I dunno if this is necessary but just in case for the door to stay unlocked until the delay time is reached
    }
    return currentState;
}


const char* getStateName(State state) { //defining names for the states
    switch (state) {
        case STATE_LOCKED:   return "Locked";
        case STATE_UNLOCKED: return "Unlocked";
    }
}
const char* getInputName(Input input) { //defining names for the inputs 
    switch (input) {
        case INPUT_QR_VALID:   return "QR Valid";
        case INPUT_QR_INVALID: return "QR Invalid";
        case INPUT_DELAY:    return "Delay";
    }
}
int main() {
    State currentState = STATE_LOCKED; //Initial state is locked

    Input inputs[] = { //now in this array there are all the inputs put randomly, im just assuming that we got this sequence of inputs
        INPUT_QR_INVALID, 
        INPUT_QR_VALID,
        INPUT_DELAY,
        INPUT_QR_VALID,
        INPUT_QR_INVALID,
        INPUT_DELAY,
        
    }; //u can try to change the sequence or just keep 1 of them to see what happens

    int A = sizeof(inputs) / sizeof(inputs[0]); // Calculating how many inputs are in the array

    for (int i = 0; i < A; i++) { //a for loop that goes through the array i gave, up there, it will go one by one and print the state and the input
        printf("Current State: %s, Input: %s\n", getStateName(currentState), getInputName(inputs[i])); //print the state and the input

        currentState = transition(currentState, inputs[i]); //now we process the input based on the transition we defined, like what will happen based on the input

        if (currentState == STATE_UNLOCKED) {
            printf("Access granted. Waiting 10 seconds before relocking.\n"); //a short note for the user to know he/she has 10 seconds to pass before relocking the door
        }
    }

    return 0;
}
