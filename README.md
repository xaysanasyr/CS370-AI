# CS370-AI
Current/Emerging Trends in Computer Science

Briefly explain the work that you did on this project: What code were you given? What code did you create yourself?
For this project we were given about 75% of the code in that block to implement a treasure hunting pirate to solve a puzzle in the best path possible, over multiple runs and learning to achieve a high win rate in the less amount of time.
This code was created with the help of my professor; #TODO: Complete the Q-Training Algorithm Code Block

    use_free_cells = True  

    for epoch in range(n_epoch):
        # Agent_cell = randomly select a free cell
        if use_free_cells:
            agent_cell = random.choice(qmaze.free_cells)
        else:
            agent_cell = np.random.randint(0, high=7, size=2)
        
        # Reset the maze with the agent set to the above position
        qmaze.reset(agent_cell)
        
        # envstate = Environment.current_state
        env_state = qmaze.observe()
        loss = 0
        episode_count = 0
        
        # While the game is not over
        while qmaze.game_status() == 'not_over':
            prev_env_state = env_state
            valid_actions = qmaze.valid_actions()
            
            # Decide whether to explore or exploit
            if np.random.rand() < epsilon:
                action = random.choice(valid_actions)  # Choose a random valid action
            else:
                action = np.argmax(experience.predict(env_state))  #Choose the best action according to the model
            
            # Perform the action and get the new state, reward, and game status
            env_state, reward, game_status = qmaze.act(action)
            
            episode_count += 1
            
            # Record the experience
            episode = [prev_env_state, action, reward, env_state, game_status]
            
            # Store the episode in the experience replay
            experience.remember(episode)
            
            # Train the model with the experiences
            inputs, targets = experience.get_data(data_size=data_size)
            model.fit(inputs, targets, epochs=8, batch_size=24, verbose=0)
            
            # Evaluate the model's performance (loss)
            loss = model.evaluate(inputs, targets, verbose=0)
            
            if game_status == "win":
                win_history.append(1)
                win_rate = sum(win_history) / len(win_history)
                break
                
            elif game_status == "lose":
                win_history.append(0)
                win_rate = sum(win_history) / len(win_history)
                break
Connect your learning from throughout this course to the larger field of computer science:

What do computer scientists do and why does it matter?
A computer scientist studies and improves technology by creating or updating algorithms, writing code, and designing models to solve problems. They might work in universities, research labs, or tech companies. Their job includes running experiments, upgrading computer systems, improving hardware, and working with engineers to develop new technology. They also share their findings with others, teach and train future scientists, and use data to come up with practical solutions, often by building databases to store important information. (Source: Indeed Editorial Team. (n.d.). What is a computer scientist? Indeed Career Guide. Retrieved August 25, 2024, from https://www.indeed.com/career-advice/finding-a-job/what-is-a-computer-scientist)

How do I approach a problem as a computer scientist?
As a computer scientist, I would approach a problem by first clearly defining it and understanding what needs to be solved. Then, I would research existing solutions and analyze the problem from different angles. After that, I would design a strategy or algorithm to solve it, breaking it down into smaller steps. Finally, I would turn my plan into code and ensure it works correctly by testing and debugging.

What are my ethical responsibilities to the end user and the organization?
As a computer scientist, I know it's important to protect users' data by keeping it safe and secure. I would do this by using encryption to prevent unauthorized access and making sure only the right people can see sensitive information. I would also make sure to only collect the data I really need and always get permission from users before doing so. Lastly, I would follow laws from the  GDPR or CCPA and make sure to be open about how data is used to build trust with users.
