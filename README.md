MACHINE LEARNING 
import copy

def candidate_elimination(concepts, target):

    # Initialize specific and general boundary
    specific_h = concepts[0].copy()
    general_h = [["?" for _ in range(len(specific_h))]]

    for i, h in enumerate(concepts):

        if target[i] == "Yes":   # Positive example
            for x in range(len(specific_h)):
                if h[x] != specific_h[x]:
                    specific_h[x] = "?"
                    general_h[0][x] = "?"

        if target[i] == "No":    # Negative example
            for x in range(len(specific_h)):
                if h[x] != specific_h[x]:
                    general_h.append(specific_h.copy())
                    general_h[-1][x] = "?"
                else:
                    general_h.append(["?" for _ in range(len(specific_h))])

    # Remove duplicate hypotheses
    general_h = [list(x) for x in set(tuple(x) for x in general_h)]

    return specific_h, general_h


# Example dataset
concepts = [
    ['Sunny', 'Warm', 'Normal', 'Strong', 'Warm', 'Same'],
    ['Sunny', 'Warm', 'High', 'Strong', 'Warm', 'Same'],
    ['Rainy', 'Cold', 'High', 'Strong', 'Warm', 'Change'],
    ['Sunny', 'Warm', 'High', 'Strong', 'Cool', 'Change']
]

target = ['Yes', 'Yes', 'No', 'Yes']

s_final, g_final = candidate_elimination(concepts, target)

print("Final Specific Hypothesis:", s_final)
print("Final General Hypothesis:", g_final)
