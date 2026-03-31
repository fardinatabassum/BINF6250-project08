# Introduction
This project presents a robust implementation of a Hidden Markov Model (HMM) utilizing the Viterbi Algorithm for sequence decoding.
The structure is built on the concept of Object-Oriented Programming, using a base `HMM` class for data management and a `Viterbi` subclass to encapsulate the decoding logic.
This modular approach ensures that the model is both scalable and maintainable for different biological datasets.

Our implementation utilizes Dynamic Programming to solve the problem of finding the most likely path through a hidden state space.
Rather than using computationally intensive recursion, the algorithm employs iterations to fill a Viterbi matrix in $O(N \cdot K^2)$ time.

# Pseudocode
```
Algorithm Viterbi (observations, states, initial_probs, transitions_probs, emission_probs)

    # Initialization
    Create viterbi_matrix of size [states] x [sequence_length]
    Create traceback_matrix (Ptr) of size [states] x [sequence_length]
    
    Convert all probabilities to log_Scale
    
    For each state 's':
        viterbi_matrix[s][0] = log_initial[s] + log_emission[s][Observation[0]]
        traceback_matrix[s][0] = none

    # Iteration Step
    For each time step 't' from 1 to sequence_length - 1:
        For each current_state: 
            best_prob = -infinity
            best_previous_state =  none
            
            #evaluate all possible paths from the previous state
			For each previous state: 
			    transition_score = viterbi_matrix[previous_state][t-1] + log_transition[previous_state][current_state] + log_emission[current_state][Observation_sequence[t]]
			
			If transition_score > best_prob:
                best_prob = transition_score
                best_previous_state = previous_state
            
            # Update viterbi_matrix with best_prob and current observation
            viterbi_matrix[current_state][t] = best_prob + log_emission[current_state][Observation_sequence[t]]
            
            #Update traceback matrix with the ptr
		    traceback_Mmtrix[current_state][t] = best_previous_state

        # Termination
	    Identify ‘final_state’ with the highest value in the viterbi_matrix

        # Traceback
	    # Initialize result path
        result_path = []
        # Start at final state and move backward from t = N-1 to t = 1 using traceback pointers
	    For each obs_index ‘t’ from Observation_Sequence Length -1 to 1:
		    final_state = traceback_matrix[final_state][t]
		    append final_state to result_path
        
        # Reverse the path list to return the optimal sequence in chronological order
        Return result_path
```

# Successes
* Our model was successfully able to detect state transitions
* We successfully implemented log-space arithmetic. This allowed the model to process long DNA sequences without losing data precision.
* We were able to use Object-Oriented Programming to create an HMM base class and a Viterbi subclass to separate the logic from data handling, making our code more reusable.
* We optimized the algorithm using Dynamic Programming and an iterative approach rather than recursion to avoid computational limitations.

# Struggles
* At first, we did not know if our model was working, as we had just gotten all G states, but we then realized that was the expected outcome. We then further verified our model using different tests with adjusted probabilities.
* We also had to figure out that the path is reconstructed in 3' to 5' order and requires a manual reversal to get the actual path.
* We also had to resolve TypeErrors caused by attempting to use lists as dictionary keys for state labels.
* There was also some confusion about how often we were applying the emission probabilities, as we did end up using them redundantly at first.

# Personal Reflections
## Group Leader
**Fardina Tabassum -** This week's project was the one I was the most excited about implementing. It was overwhelming at first, but thanks to my teammates and after a quick recap of the math behind HMMs, we were able to
break down the pseudocode in detail, which helped with our overall workflow. It was a bit difficult to translate the mathematical recurrences into Dynamic Programming, specifically, figuring out how to implement the iterations in log-space. 
I also hit a major hurdle when the initial path predictions were inconsistent. I eventually figured out I was redundantly using the emission probabilities by applying them twice within our nested loops. We were also unsure if our model was working at all at first, as we were all getting the same states, but then we figured out that it was the expected outcome. This project for me was also another good refresher for object-oriented programming. It was also interesting for me to see how the path changes when the probabilities of transition and emission are adjusted. I also struggled a bit understanding the traceback as I had not realized that the optimal path is identified at the end of the sequence first and requires a manual reversal to match the standard biological 5' to 3' orientation. Overall, my team and I were able to spend a good amount of time diagnosing issues with our initial logic and ensuring we have an ideal structure so that our code is more robust and reusable for future implementations.

## Other member
**Meghana Ravi -** This project was a little difficult for me to follow in terms of the implementation at first. I understood the concept of HMMs and the Viterbi algorithm well, but figuring out how to make the implementation as general as possible confused me a lot. Working it out with my group members really helped, and once we got started on the pseudocode I was able to follow much better. Going through the design and discussing how different parts of the algorithm connected helped me understand how to structure the code for reuse. Making code reusable for other algorithms or future use isn't something I've done before so this project was a great learning experience for me.

**Connor Crawford -**

# Generative AI Appendix
As per the syllabus
