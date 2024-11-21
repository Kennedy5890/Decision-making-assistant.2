# Decision-Making Assistant

def get_user_input(prompt):
    """Safely gets user input and handles basic validation."""
    while True:
        try:
            response = input(prompt)
            return response
        except KeyboardInterrupt:
            print("\nExiting.")
            exit()
        except Exception as e:
            print(f"Error: {e}, please try again.")

def get_float(prompt, min_value=0, max_value=10):
    """Get a float input from the user with validation."""
    while True:
        try:
            value = float(get_user_input(prompt))
            if min_value <= value <= max_value:
                return value
            else:
                print(f"Value must be between {min_value} and {max_value}.")
        except ValueError:
            print("Invalid input, please enter a number.")

def weighted_decision_making():
    print("Welcome to the Decision-Making Assistant!")
    print("This assistant will help you choose the best option based on weighted criteria.")

    # Step 1: Define the criteria
    criteria = []
    print("\nStep 1: Enter your decision criteria (e.g., cost, time, quality).")
    print("Type 'done' when finished.")
    while True:
        criterion = get_user_input("Enter a criterion: ").strip()
        if criterion.lower() == 'done':
            if len(criteria) < 2:
                print("Please enter at least two criteria.")
                continue
            break
        elif criterion:
            criteria.append(criterion)
    
    # Step 2: Assign weights to criteria
    print("\nStep 2: Assign weights to each criterion (weights must add up to 1).")
    weights = {}
    total_weight = 0
    for criterion in criteria:
        while True:
            weight = get_float(f"Enter weight for {criterion} (remaining weight: {1 - total_weight:.2f}): ", 0, 1)
            if total_weight + weight <= 1:
                weights[criterion] = weight
                total_weight += weight
                break
            else:
                print("Total weights cannot exceed 1.")
    if total_weight != 1:
        print("\nWeights do not add up to 1, adjusting automatically...")
        factor = 1 / total_weight
        for criterion in weights:
            weights[criterion] *= factor

    # Step 3: Rate options
    options = []
    print("\nStep 3: Enter your options (e.g., 'Option A', 'Option B').")
    print("Type 'done' when finished.")
    while True:
        option = get_user_input("Enter an option: ").strip()
        if option.lower() == 'done':
            if len(options) < 2:
                print("Please enter at least two options.")
                continue
            break
        elif option:
            options.append(option)

    scores = {option: {} for option in options}

    print("\nStep 4: Rate each option for every criterion (scale: 0-10).")
    for option in options:
        for criterion in criteria:
            scores[option][criterion] = get_float(f"Rate {option} for {criterion} (0-10): ", 0, 10)

    # Step 5: Calculate and display results
    print("\nCalculating results...")
    results = {}
    for option in
