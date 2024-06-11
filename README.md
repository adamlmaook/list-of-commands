# list of commands
 The files are in visual studio code and since this is my first time adding this to github, im going to include the code lines in a READ ME as well.Codes are for python and as the name suggest, its a list of commands i had the code perform for ease of use and i plan to add more commands in the future.. As for the list of commands, you just gonna have to check it out yourself  ;P

import datetime
from email.headerregistry import HeaderRegistry
from multiprocessing import Value
from optparse import Option
from re import M, T
import webbrowser
import os
import tkinter as tk
from tkinter import filedialog
import sys
import platform
import config
import requests
import json
import pyfiglet
from termcolor import colored
import random
from colorama import Fore, Style
import math
import cmath

# Function to display the current time
def display_time():
    current_time = datetime.datetime.now().strftime("%Hhr %M min %Ssec")
    print("Current time is:", current_time)

# Function to display the current date in dd/mm/yyyy format
def display_date():
    current_date = datetime.date.today().strftime("%d/%m/%Y")
    print("Current date is:", current_date)

# Function to remind a task and save it in a file
def remind():
    task = input("Sure, what do you want me to remind? ")

    try:
        import tkinter as tk
        from tkinter import filedialog

        root = tk.Tk()
        root.attributes('-topmost', True)
        root.withdraw()

        folder_path = filedialog.askdirectory(title="Select Folder to Save Reminder")

        # Generate the file name from the task
        file_name_input = task.replace(' ', '_')  # Replace spaces with underscores or any other desired logic

        file_path = filedialog.asksaveasfilename(
            defaultextension=".txt",
            filetypes=(("Text files", "*.txt"), ("All files", "*.*")),
            initialdir=folder_path,  # Set the initial directory
            initialfile=file_name_input  # Use the generated file name as the initial file name
        )

        if file_path:
            with open(file_path, "w") as file:
                file.write(task + "\n")
            print("Task saved in " + file_name_input + ".txt")
            print("File saved successfully.")
        else:
            print("Save operation canceled.")
    except Exception as e:
        print("An error occurred:", str(e))
def search_youtube(video_name):
    search_url = f"https://www.youtube.com/results?search_query={video_name.replace(' ', '+')}"
    if platform.system() == "Windows":
        webbrowser.open(search_url, new=2)
    else:
        webbrowser.open(search_url)
    print(f"Searching YouTube for '{video_name}'...")

# Function to search the web for a keyword
def search_web(keyword, selected_engine):
    search_url = ""
    if selected_engine.lower() == "1" or selected_engine.lower() == "google":
        search_url = f"https://www.google.com/search?q={keyword.replace(' ', '+')}"
    elif selected_engine == "2" or selected_engine.lower() == "bing":
        search_url = f"https://www.bing.com/search?q={keyword.replace(' ', '+')}"
    elif selected_engine == "3" or selected_engine.lower() == "yahoo":
        search_url = f"https://www.yahoo.com/search?q={keyword.replace(' ', '+')}"
    elif selected_engine == "4" or selected_engine.lower() == "baidu":
        search_url = f"https://www.baidu.com/search?q={keyword.replace(' ', '+')}"
    elif selected_engine == "5" or selected_engine.lower() == "yandex":
        search_url = f"https://www.yandex.com/search?q={keyword.replace(' ', '+')}"
    elif selected_engine == "6" or selected_engine.lower() == "ask.com":
        search_url = f"https://www.ask.com/web?q={keyword.replace(' ', '+')}"
    elif selected_engine == "7" or selected_engine.lower() == "AOL Search":
        search_url = f"https://www.search.aol.com/search?q={keyword.replace(' ', '+')}"
    elif selected_engine == "8" or selected_engine.lower() == "ecosia":
        search_url = f"https://www.ecosia.com/search?q={keyword.replace(' ', '+')}"
    elif selected_engine == "9" or selected_engine.lower() == "search":
        search_url = f"https://www.search.com/search?q={keyword.replace(' ', '+')}"
    if search_url:
        if platform.system() == "Windows":
            webbrowser.open(search_url, new=2)
        else:
            webbrowser.open(search_url)
        print(f"Searching {selected_engine} for '{keyword}'...")

# Function to search for files on all available drives, including USB drives
def search_files(file_name):
    found = False
    search_results = []  # Store search results in a list
    
    drives = ["C:", "D:", "E:", "F:", "G:", "H:", "I:", "J:", "K:"]
    
    for drive in drives:
        if os.path.exists(drive):
            for root, dirs, files in os.walk(drive):
                for file in files:
                    if file_name.lower() in file.lower():
                        found = True
                        search_results.append(os.path.join(root, file))
    
    # Search for USB drives
    for drive in range(1, 27):
        drive_letter = chr(ord('A') + drive - 1) + ":\\"
        if os.path.exists(drive_letter):
            for root, dirs, files in os.walk(drive_letter):
                for file in files:
                    if file_name.lower() in file.lower():
                        found = True
                        search_results.append(os.path.join(root, file))
    
    if not found:
        print("No search results found.")
    else:
        # Display search results with numbers
        for i, result in enumerate(search_results, start=1):
            print(f"{i}. {result}")

        # Ask if the user wants to open a file or its location
        open_choice = input("Do you want to open the file or its location? Press 1 for file, 2 for location (else invalid): ").lower()
        if open_choice == "1":
            try:
                # Ask for the number of the file to open
                file_number = int(input("Enter the number of the file to open: ")) - 1
                if 0 <= file_number < len(search_results):
                    file_to_open = search_results[file_number]
                    if platform.system() == "Windows":
                        os.startfile(file_to_open)  # Open the file
                    else:
                        print(f"Opening {file_to_open}...")
                else:
                    print("Invalid number.")
            except ValueError:
                print("Invalid input. Please enter a valid number.")
        elif open_choice == "2":
            try:
                # Ask for the number of the file location to open
                file_location_number = int(input("Enter the number of the file location to open: ")) - 1
                if 0 <= file_location_number < len(search_results):
                    file_location = os.path.dirname(search_results[file_location_number])
                    if platform.system() == "Windows":
                        os.startfile(file_location)  # Open the file location
                    else:
                        print(f"Opening {file_location}...")
                else:
                    print("Invalid number.")
            except ValueError:
                print("Invalid input. Please enter a valid number.")
        else:
            print("Invalid choice.")

# Function to delete a file using a file dialog
def delete_file(file_path):
    try:
        os.remove(file_path)
        print(f"File '{file_path}' deleted successfully.")
    except FileNotFoundError:
        print(f"File not found: {file_path}")
    except Exception as e:
        print(f"An error occurred while deleting the file: {str(e)}")

# Function to delete a folder using a folder dialog
def delete_folder(folder_path):
    try:
        os.rmdir(folder_path)
        print(f"Folder '{folder_path}' deleted successfully.")
    except FileNotFoundError:
        print(f"Folder not found: {folder_path}")
    except OSError as e:
        print(f"An error occurred while deleting the folder: {str(e)}")

# Function to delete a file using a file dialog
def delete_file_dialog():
    try:
        root = tk.Tk()
        root.attributes('-topmost', True)
        root.withdraw()
        
        file_path = filedialog.askopenfilename(title="Select File to Delete")
        
        if file_path:
            delete_file(file_path)
    except Exception as e:
        print(f"An error occurred: {str(e)}")

# Function to delete a folder using a folder dialog
def delete_folder_dialog():
    try:
        root = tk.Tk()
        root.attributes('-topmost', True)
        root.withdraw()

        folder_path = filedialog.askdirectory(title="Select Folder to Delete")

        if folder_path:
            files_in_folder = os.listdir(folder_path)
            if not files_in_folder:
                delete_folder(folder_path)
            else:
                print(f"The folder '{folder_path}' contains {len(files_in_folder)} files.")
                confirm_delete = input("Are you sure you want to delete it along with its files? (yes/no): ").lower()
                if confirm_delete == "yes":
                    for root, dirs, files in os.walk(folder_path, topdown=False):
                        for file in files:
                            file_path = os.path.join(root, file)
                            delete_file(file_path)
                    os.rmdir(folder_path)
                    print(f"Folder '{folder_path}' deleted along with its files.")
                else:
                    print("Deletion canceled.")
    except Exception as e:
        print(f"An error occurred: {str(e)}")

def get_weather(api_key, city):
    base_url = "http://api.openweathermap.org/data/2.5/weather"
    forecast_url = "http://api.openweathermap.org/data/2.5/forecast"
    params = {
        "q": city,
        "appid": api_key,
        "units": "metric",  # You can change this to "imperial" for Fahrenheit
    }

    try:
        # Fetch current weather
        response = requests.get(base_url, params=params)
        response.raise_for_status()
        current_data = response.json()

        if response.status_code == 200:
            # Extract relevant information from the current weather API response
            weather_description = current_data["weather"][0]["description"]
            temperature = current_data["main"]["temp"]
            humidity = current_data["main"]["humidity"]
            wind_speed = current_data["wind"]["speed"]
            sunrise_time = datetime.datetime.fromtimestamp(current_data["sys"]["sunrise"], datetime.timezone.utc).strftime("%H:%M:%S")
            sunset_time = datetime.datetime.fromtimestamp(current_data["sys"]["sunset"], datetime.timezone.utc).strftime("%H:%M:%S")

            # Print the current weather information
            print(f"Weather in {city}: {weather_description.capitalize()}")
            print(f"Temperature: {temperature}°C")
            print(f"Humidity: {humidity}%")
            print(f"Wind Speed: {wind_speed} m/s")
            print(f"Sunrise: {sunrise_time}")
            print(f"Sunset: {sunset_time}")

            # Fetch three-day weather forecast
            forecast_params = {
                "q": city,
                "appid": api_key,
                "units": "metric",
            }

            forecast_response = requests.get(forecast_url, params=forecast_params)
            forecast_response.raise_for_status()
            forecast_data = forecast_response.json()

            if forecast_response.status_code == 200:
    # Extract and print the forecast for the next three days
                print("\nFive-day Weather Forecast:")
                unique_dates = set()  # To store unique dates
                #for day in forecast_data["list"][:3]:
        # Convert timestamp to local time zone and format the date
                for day in forecast_data["list"]:
        # Convert timestamp to local time zone and format the date
                    date = datetime.datetime.fromtimestamp(day["dt"], datetime.timezone.utc).astimezone().strftime("%Y-%m-%d")
        # Skip duplicates
                    if date in unique_dates:
                        continue

                    unique_dates.add(date)  # Add the date to the set to track uniqueness

        # Extract other information
                    temperature = day["main"]["temp"]
                    description = day["weather"][0]["description"]

        # Print the weather information
                    print(f"{date}: {description.capitalize()}, Temperature: {temperature}°C")

            else:
                print(f"Failed to retrieve weather forecast. Status code: {forecast_response.status_code}")

        else:
            print(f"Failed to retrieve current weather information. Status code: {response.status_code}")

    except requests.exceptions.HTTPError as http_err:
        print(f"HTTP error occurred: {http_err}")
    except Exception as e:
        print(f"An error occurred: {str(e)}")

# Function to fetch and display a random joke
def get_joke():
    joke_api_url = "https://v2.jokeapi.dev/joke/Any"  # Adjust the URL based on the API documentation

    try:
        response = requests.get(joke_api_url)
        response.raise_for_status()

        joke_data = response.json()

        if joke_data["type"] == "twopart":
            joke_setup = joke_data["setup"]
            joke_delivery = joke_data["delivery"]
            print(f"Joke:\n{joke_setup}\n{joke_delivery}")  # Fix variable names
        elif joke_data["type"] == "single":
            joke_content = joke_data["joke"]
            print(f"Joke:\n{joke_content}")
        else:
            print("Unable to fetch a joke. Please try again later.")

    except requests.exceptions.RequestException as e:
        print(f"Error fetching joke: {e}")

def get_fact():
    print("Sure, what type of fact do you want? I have:")
    print("1. Fun Fact\n2. Disturbing Fact\n3. Scary Fact\n4. Mind-blowing Fact\n5. Bizarre Fact\n6. Heartwarming Fact\n7. Mind-boggling Fact\n8. Contradictory Fact")

    fact_type = int(input("Enter the number corresponding to the type of fact you want: "))

    fun_facts = [
        "Bananas are berries, but strawberries aren't.",
        "Honey never spoils; archaeologists have found pots of honey in ancient Egyptian tombs that are over 3,000 years old and still perfectly edible.",
        "Cows have best friends and can become stressed when they are separated.",
        "The shortest war in history was between Britain and Zanzibar on August 27, 1896. Zanzibar surrendered after 38 minutes.",
        "The Eiffel Tower can be 15 cm taller during the summer, due to the expansion of the iron when it's hot.",
        "Wombat poop is cube-shaped.",
        "Octopuses have three hearts: two pump blood to the gills, and one pumps it to the rest of the body.",
        "A group of flamingos is called a FLAMBOYANCE",
        "A small child could swim through the veins of a blue whale.",
        "The inventor of the frisbee was turned into a frisbee. Walter Morrison, the inventor, was cremated, and his ashes were turned into a frisbee after he passed away."
    ]
    disturbing_fact = [
        "The averages person walks past 36 murders in their lifetime.",
        "Some tumours can grow teeth and hair.",
        "If you fell into a black hole, you could see the start and the end of the universe, Big Bang and all.",
        " Research on New York subway stations in 2008-2009 showed that 15% of the air molecules were actually human skin. What's more disturbing is that while most of the skin came from people's heels and heads, 12% was from armpits, belly buttons, inner ears, and backsides.",
        "Peanut butter and other packaged foods can have rodent hair, feces, and inset parts in it.",
        "Dogs love squeaky toys because they sound like wounded animals.",
        "The CPR dummy face template is based on a dead 16-year-old girl’s face. Her body was pulled from the River Seine in the 1880s, and her cause of death and identity were never discovered. A mortician was so ‘entranced’ by her peaceful expression that he made a plaster mask of her, which became the face of CPR dummies and health training equipment.",
        "Siberian bears sometimes dig up dead bodies for food for food, and use cemeteries as 'refrigerators'.",
        ]
    scary_fact = [
        "If you are a healthy 20 year old,you have around 2860 weeks before you die. (Just 2860 Sundays).",
        "The average person will be less successful than they think.",
        "About 153,000 people die on your birthday.",
        "Seals have been known to rape penguins.",
        "If you took all of the world's spiders and let them out in the Netherlands, they would consume the country's population in three days.",
        "One in fifty of us is walking around with a brain aneurysm. It just hasn't ruptured.",
        "The Colombian serial killer Pedro Alonso Lopez, who is known as the Monster of the Andes, raped and murdered over 300 girls from Ecuador, Peru and Colombia. However, after he was caught and imprisoned for 18 years, he was put in a psychiatric hospital. There he was reviewed, declared to be sane and was set free, in spite of his blatant avowal that he fully intends to kill again. Ever since his release in 1998, nobody knows where he is or what he’s doing.",
        "In 2012, scientists found 1,458 new species of bacteria living in the belly button. Everyone's belly button ecology is unique like a fingerprint, and one volunteer's belly button harbored bacteria that had previously been found only in soil from Japan where he had never been.",
        "A person suffering from Cotard’s Syndrome believes he/she is dead. Cotard’s syndrome comprises any one of a series of delusions that range from a belief that one has lost organs, blood, or body parts to insisting that one has lost one’s soul or is dead.",
        "If you're looking at a Victorian photo and one of the subjects in the photo is clearer than the rest, they're probably dead.",
        "An octopus is flexible enough to enter your mouth, navigate your digestive system and leave through your anus.",
        "60% of the UK population feels like no one really loves them."
        ]
    mind_blowing_fact = [
        "Froot Loops are all the same flavor",
        "More French soldiers died during World War I than American soldiers during all of U.S. history",
        "It’s totally legal to escape from prison in Mexico",
        "The longest war in history was between Britain and Zanzibar on August 27, 1896. Zanzibar surrendered after 38 minutes",
        "A strawberry isn’t actually a berry—but a watermelon is",
        "If you’re shot by a sniper, you’ll be dead before you hear the gun",
        "Yoda and Miss Piggy were voiced by the same person",
        "The Las Vegas Strip isn’t in Las Vegas",
        "The largest desert in the world is covered in snow",
        "“Banana” flavoring is based on an extinct type of banana",
        "Every two minutes people take more photos than were taken in the entire 19th century",
        "The raptor sounds in Jurassic Park are actually mating tortoises"
        ]
    bazarre_fact = [
        "All the electricity powering the internet weighs the same as an apricot.",
        "A hippo’s jaw opens wide enough to fit a sports car inside.",
        "It would take 19 minutes to fall from the North Pole to Earth’s core.",
        "The packaging problems of round fruit can be solved by making them square. In Korea, some apples are grown in plastic moulds so they take on a square shape.",
        "Calculations suggest that 136 billion sheets of A4 paper would be needed to print out the entire world wide web. If the printouts were piled up, the stack would be taller than Earth.",
        "The most forceful rollercoaster in the world is “Tower of Terror” at Gold Reef City in Johannesburg, South Africa. At the bottom of the ride’s huge drop, people experience a G-force of 6.3g, twice the G-force of a space shuttle launch.",
        "It’s estimated that the typical pencil has enough graphite to draw a line 56 km (35 miles) long, or 20 times the length of the Golden Gate Bridge.",
        " Six generations back, you have 64 great-great-great-grandparents."
        ]
    heart_warming_fact = [
        "Sperm whales never forget a friend.",
        "An injured Olympic runner crossed the finish line with his dad.",
        "In Washington State, some prisoners rehabilitate cats.",
        "One man's blood saved more than two million children.",
        "Penguins propose to their mates.",
        "Dolphins are known to save people from sharks",
        "Rats laugh when you tickle them.",
        "Winnie the Pooh cheers up sick children with phone calls.",
        "Some animals make it off the endangered list.",
        "A motorcycle gang helps children cope with trauma.",
        "The actors who voice Mickey and Minnie Mouse got married in real life.",
        "Vikings gave kittens to new brides."
        ]
    mind_boggling_fact = [
        "The human brain generates more electrical impulses in a single day than all the telephones in the world combined.",
        "Every star you see in the night sky is larger and brighter than our sun, yet they appear tiny due to their immense distance from Earth.",
        "A teaspoon of neutron star material would weigh about 6 billion tons, roughly the same as Mount Everest.",
        "There are more possible iterations of a game of chess than there are atoms in the observable universe.",
        "A day on Venus is longer than a year on Venus, as it takes Venus 243 Earth days to complete one rotation on its axis, but only 225 Earth days to orbit the Sun.",
        "Light from the Sun takes approximately 8 minutes and 20 seconds to reach Earth, meaning we see the Sun as it was over 8 minutes ago.",
        "Octopuses have three hearts and blue blood due to the copper-based molecule, hemocyanin, used to transport oxygen.",
        "The average cloud weighs about 1.1 million pounds, roughly equivalent to 100 elephants.",
        "The Great Wall of China is not visible from space with the naked eye, contrary to popular belief.",
        "Bananas are berries, but strawberries are not."
        ]
    contradictory_fact = [
        "While the universe appears infinite in its vastness, it's also finite in age, having emerged from the Big Bang approximately 13.8 billion years ago.",
		"Despite being the closest planet to the Sun, Mercury has ice at its poles due to the lack of atmosphere to retain heat.",
		"The Sahara Desert, known for its scorching temperatures, can also experience freezing temperatures at night due to its lack of humidity.",
		"Despite being the largest planet in our solar system, Jupiter spins faster on its axis than any other planet, completing a rotation in less than 10 hours.",	
		"The deepest point on Earth, the Mariana Trench, is located in the Pacific Ocean, where water pressure is intense enough to crush a submarine, yet life still thrives in its depths.",
		"Black holes, known for their powerful gravitational pull that not even light can escape, also emit radiation known as Hawking radiation, gradually losing mass over time.",
		"The coldest temperature ever recorded on Earth was −128.6°F (−89.2°C) in Antarctica, while the hottest temperature ever recorded was 134°F (56.7°C) in Death Valley, California.",
		"The Amazon Rainforest, often associated with abundant rainfall, also experiences a dry season where rainfall is significantly reduced, leading to drought conditions.",
		"While the Arctic Circle experiences periods of continuous daylight during summer months, it also undergoes periods of continuous darkness during the winter, known as the polar night.",
		"Despite being the driest place on Earth, the Atacama Desert in Chile is also home to microbial life adapted to extreme conditions, thriving in the hyper-arid environment."
        ]


    if fact_type == 1:
        random_fun_fact = random.choice(fun_facts)
        print("\nHere's a Fun Fact for you:")
        print(random_fun_fact)
    
    elif fact_type == 2:
        random_disturbing_fact = random.choice(disturbing_fact)
        print("\nHere's a Disturbing Fact for you:")
        print(random_disturbing_fact)

    elif fact_type == 3:
        random_scary_fact = random.choice(scary_fact)
        print("\nHere's a Scary Fact for you:")
        print(random_scary_fact)

    elif fact_type == 4:
        random_mind_blowing_fact = random.choice(mind_blowing_fact)
        print("\nHere's a Mind Blowing Fact for you:")
        print(random_mind_blowing_fact)

    elif fact_type == 5:
        random_bizarre_fact = random.choice(bazarre_fact)
        print("\nHere's a Bizarre Fact for you:")
        print(random_bizarre_fact)
    
    elif fact_type == 6:
        random_heart_warming_fact = random.choice(heart_warming_fact)
        print("\nHere's a Heart Warming Fact for you:")
        print(random_heart_warming_fact)
    
    elif fact_type == 7:
        random_mind_boggling_fact = random.choice(mind_boggling_fact)
        print("\nHere's a Mind Bogglinng Fact for you:")
        print(random_mind_boggling_fact)

    elif fact_type == 8:
        random_contradictory_fact = random.choice(contradictory_fact)
        print("\nHere's a Contradictory Fact for you:")
        print(random_contradictory_fact)

    else:
        print("Invalid choice. Please enter a number between 1 and 8.")

def get_ascii_art(text, font):
    try:
        ascii_art = pyfiglet.figlet_format(text, font=font)
        return ascii_art
    except pyfiglet.FigletError as e:
        print(f"Error generating ASCII art: {e}")

def create_tkinter_window():
    root = tk.Tk()
    root.title("Simple Tkinter Window")
    
    label = tk.Label(root, text="This is a simple Tkinter window.")
    label.pack(padx=20, pady=20)

    root.mainloop()

# List of available fonts
available_fonts = [
    "block",
    "slant",
    "big",
    "script",
    "small",
    "banner",
    "digital"
]

def rock_paper_scissors_game():
    print("Welcome to Rock, Paper, Scissors!")
    print("Enter your choice: 'rock', 'paper', or 'scissors'.")

    choices = ['rock', 'paper', 'scissors']
    computer_choice = random.choice(choices)

    user_choice = input("Your choice: ").lower()

    print(f"Computer's choice: {computer_choice}")

    if user_choice in choices:
        if user_choice == computer_choice:
            print("It's a tie!")
        elif (
            (user_choice == 'rock' and computer_choice == 'scissors') or
            (user_choice == 'scissors' and computer_choice == 'paper') or
            (user_choice == 'paper' and computer_choice == 'rock')
        ):
            print("You win!")
        else:
            print("You lose!")
    else:
        print("Invalid choice. Please enter 'rock', 'paper', or 'scissors'.")

def print_board(board):
    print("   1   2   3 ")
    print("  -------------")
    for i, row in enumerate(board):
        print(chr(ord('A') + i), "|", end="")
        for cell in row:
            print(f" {cell} |", end="")
        print("\n  -------------")

def check_winner(board, player):
    for i in range(3):
        if all(cell == player for cell in board[i]) or all(row[i] == player for row in board):
            return True
    if all(board[i][i] == player for i in range(3)) or all(board[i][2 - i] == player for i in range(3)):
        return True
    return False

def is_board_full(board):
    return all(all(cell != ' ' for cell in row) for row in board)

def tic_tac_toe_game():
    print("Welcome to Tic Tac Toe!")
    
    while True:
        board = [[' ' for _ in range(3)] for _ in range(3)]
        current_player = 'X'
        
        # Ask whether to play against AI or human
        play_against_ai = input("Enter 1 to play against AI, 2 to play against human, or 0 to go back: ")
        
        if play_against_ai == '0':
            break  # Go back to main menu
        elif play_against_ai == '1':
            play_against_ai = True
        elif play_against_ai == '2':
            play_against_ai = False
        else:
            print("Invalid choice. Please enter 1, 2, or 0.")
            continue

        while True:
            print_board(board)

            if play_against_ai and current_player == 'O':
                # AI's move (simple random move for demonstration purposes)
                import random
                row_input = random.choice(['A', 'B', 'C'])
                col_input = str(random.randint(1, 3))
                print(f"AI chooses: {row_input}{col_input}")
            else:
                row_input = input("Enter the row (A, B, or C): ").upper()
                col_input = input("Enter the column (1, 2, or 3): ")

            # Validate column input
            if col_input.isdigit():
                col = int(col_input) - 1
            else:
                print("Invalid column input. Please enter a valid number (1, 2, or 3).")
                continue

            # Validate row input
            if row_input in {'A', 'B', 'C'}:
                row = ord(row_input) - ord('A')
            else:
                print("Invalid row input. Please enter a valid row (A, B, or C).")
                continue

            if 0 <= row < 3 and 0 <= col < 3 and board[row][col] == ' ':
                board[row][col] = current_player

                if check_winner(board, current_player):
                    print_board(board)
                    if play_against_ai and current_player == 'O':
                        print("AI wins!")
                    else:
                        print(f"Player {current_player} wins!")
                    break
                elif is_board_full(board):
                    print_board(board)
                    print("It's a tie!")
                    break

                current_player = 'O' if current_player == 'X' else 'X'
            else:
                print("Invalid move. Try again.")

# Function to perform basic arithmetic operations
def simple_calculator():
    try:
        num1 = float(input("Enter the first number: "))
        operator = input("Enter the operator (+, -, *, /): ")
        num2 = float(input("Enter the second number: "))

        if operator == "+":
            result = num1 + num2
        elif operator == "-":
            result = num1 - num2
        elif operator == "*":
            result = num1 * num2
        elif operator == "/":
            if num2 != 0:
                result = num1 / num2
            else:
                print("Error: Division by zero.")
                return
        else:
            print("Invalid operator. Please enter +, -, *, or /.")
            return

        print(f"Result: {num1} {operator} {num2} = {result}")

    except ValueError:
        print("Invalid input. Please enter valid numbers.")

# Function to calculate the area of a square
def calculate_square_area():
    try:
        num1 = float(input("Enter the length of the square: "))
        square_area = num1 * num1
        print(f"The area of the square with length {num1} is: {square_area}")

    except ValueError:
        print("Invalid input. Please enter a valid length.")

# Function to calculate the area of a rectangle
def calculate_rectangle_area():
    try:
        num1 = float(input("Enter the length of the rectangle: "))
        num2 = float(input("Enter the breadth of the rectangle: "))
        rectangle_area = num1 * num2
        print(f"The area of the rectangle with length {num1} and breadth {num2} is: {rectangle_area}")

    except ValueError:
        print("Invalid input. Please enter valid length and breadth.")

# Function to calculate the area of a triangle
def calculate_triangle_area():
    try:
        num1 = float(input("Enter the base length of the triangle: "))
        num2 = float(input("Enter the height of the triangle: "))
        triangle_area = 0.5 * num1 * num2
        print(f"The area of the triangle with base {num1} and height {num2} is: {triangle_area}")

    except ValueError:
        print("Invalid input. Please enter valid base and height.")

def calculate_circle_area():
    try:
        num1 = float(input("Enter the radius of the circle: "))
        circle_area = 3.14 * num1 * num1
        print(f"The area of the circle with radius {num1} is: {circle_area}")
    except ValueError:
        print("Invalid input. Please enter valid radius.")

def calculate_trapezoid_area():
    try:
        num1 = float(input("Enter the base1 length of the trapezoid: "))
        num2 = float(input("Enter the base2 length of the trapezoid: "))
        num3 = float(input("Enter the height of the trapezoid: "))
        trapezoid_area = 0.5 * (num1 + num2) * num3
        print(f"The area of the trapezoid with base1 {num1}, base2 {num2} and height {num3} is: {trapezoid_area}")

    except ValueError:
        print("Invalid input. Please enter valid base1, base2 and height.")

def calculate_cube_volume():
    try:
        num1 = float(input("Enter the length of the cube: "))
        cube_volume = num1 * num1 * num1
        print(f"The volume of the cube with length {num1} is: {cube_volume}")

    except ValueError:
        print("Invalid input. Please enter valid length.")

def calculate_cuboid_volume():
    try:
        num1 = float(input("Enter the length of the cuboid: "))
        num2 = float(input("Enter the breadth of the cuboid: "))
        num3 = float(input("Enter the height of the cuboid: "))
        cuboid_volume = num1 * num2 * num3
        print(f"The volume of the cuboid with length {num1}, breadth {num2} and height {num3} is: {cuboid_volume}")

    except ValueError:
        print("Invalid input. Please enter valid length, breadth and height.")

def calculate_cone_volume():
    try:
        num1 = float(input("Enter the radius of the cone: "))
        num2 = float(input("Enter the height of the cone: "))
        cone_volume = 0.33 * 3.14 * num1 * num1 * num2
        print(f"The volume of the cone with radius {num1} and height {num2} is: {cone_volume}")

    except ValueError:
        print("Invalid input. Please enter valid radius and height.")

def calculate_cylinder_volume():
    try:
        num1 = float(input("Enter the radius of the cylinder: "))
        num2 = float(input("Enter the height of the cylinder: "))
        cylinder_volume = 3.14 * num1 * num1 * num2
        print(f"The volume of the cylinder with radius {num1} and height {num2} is: {cylinder_volume}")

    except ValueError:
        print("Invalid input. Please enter valid radius and height.")

def calculate_sphere_volume():
    try:
        num1 = float(input("Enter the radius of the sphere: "))
        sphere_volume = 4 / 3 * 3.14 * num1 * num1 * num1
        print(f"The volume of the sphere with radius {num1} is: {sphere_volume}")

    except ValueError:
        print("Invalid input. Please enter valid radius.")


def calculate_prism_volume():
    try:
        num1 = float(input("Enter the length of the prism: "))
        num2 = float(input("Enter the breadth of the prism: "))
        num3 = float(input("Enter the height of the prism: "))
        prism_volume = num1 * num2 * num3
        print(f"The volume of the prism with length {num1}, breadth {num2} and height {num3} is: {prism_volume}")

    except ValueError:
        print("Invalid input. Please enter valid length, breadth and height.")

def calculate_pyramid_volume():
    try:
        num1 = float(input("Enter the length of the pyramid: "))
        num2 = float(input("Enter the breadth of the pyramid: "))
        num3 = float(input("Enter the height of the pyramid: "))
        pyramid_volume = 0.5 * num1 * num2 * num3
        print(f"The volume of the pyramid with length {num1}, breadth {num2} and height {num3} is: {pyramid_volume}")

    except ValueError:
        print("Invalid input. Please enter valid length, breadth and height.")

# Function to perform complex calculations
def complex_calculator():
    try:
        print("Choose a complex calculator option:")
        print("1. Exponentiation (a ** b)")
        print("2. Square Root (sqrt)")
        print("3. Calculate Areas")
        print("4. Calaculate Volume")
        print("5. Algebric formulas")
        print("6. Factorial (a!)")
        print("7. Convert")

        option = input("Enter the option (1, 2, 3, 4, 5 or 6): ")

        if option == "1":
            num1 = float(input("Enter the base number: "))
            num2 = float(input("Enter the exponent: "))
            result = num1 ** num2
            print(f"Result: {num1} ** {num2} = {result}")
        elif option == "2":
            num = float(input("Enter the number to find the square root: "))
            result = math.sqrt(num)
            print(f"Result: Square root of {num} = {result}")
        elif option == "3":
            calculate_areas()
        elif option == "4":
            calculate_volume()
        elif option == "5":
            calculate_algebra_formulas()
        elif option == "6":
            num = int(input("Enter the number: "))
            result = math.factorial(num)
            print(f"Result: {num}! = {result}")
        elif option == "7":
            convert()
        else:
            print("Invalid option. Please enter 1, 2, 3, 4, 5, 6 or 7 .")

    except ValueError:
        print("Invalid input. Please enter valid numbers.")


# Function to calculate areas 
def calculate_areas():
    print("Choose an option to calculate area:")
    print("1. Calculate Square Area")
    print("2. Calculate Rectangle Area")
    print("3. Calculate Triangle Area")
    print("4. Calculate Circle Area")
    print("5. Calculate Trapezoid Area")

    area_option = input("Enter the option (1, 2, or 3): ")

    if area_option == "1":
        calculate_square_area()
    elif area_option == "2":
        calculate_rectangle_area()
    elif area_option == "3":
        calculate_triangle_area()
    elif area_option == "4":
        calculate_circle_area()
    elif area_option == "5":
        calculate_trapezoid_area()
    else:
        print("Invalid option. Please enter 1, 2, 3, 4 or 5.")

def calculate_volume():
    print("Choose an option to calculate volume:")
    print("1. Calculate Cube volume")
    print("2. Calculate Cuboid volume")
    print("3. Calculate Cone volume")
    print("4. Calculate Cylinder volume")
    print("5. Calculate Sphere volume")
    print("6. Calculate Prism Volume")
    print("7. Calculate Pyramid Volume")
    
    volume_option = input("Enter the option (1, 2, 3, 4, 5, 6 or 7): ")
    if   volume_option == "1":
        calculate_cube_volume()
    elif volume_option == "2":
        calculate_cuboid_volume()
    elif volume_option == "3":
        calculate_cone_volume()
    elif volume_option == "4":
        calculate_cylinder_volume()
    elif volume_option == "5":
        calculate_sphere_volume()
    elif volume_option == "6":
        calculate_prism_volume()
    elif volume_option == "7":
        calculate_pyramid_volume()
    else:
        print("Invalid option. Please enter 1, 2, 3, 4, 5, 6 or 7.")

def calculate_algebra_formulas():
    print("Choose an equation you want to use within arametic formulas:")
    print("1.  a^2 – b^2     = (a – b)(a + b)")
    print("2.  (a + b)^2     = a^2 + 2ab + b^2")
    print("3.  a^2 + b^2     = (a + b)^2 – 2ab")
    print("4.  (a – b)^2     = a^2 – 2ab + b^2")
    print("5.  (a + b + c)^2 = a^2 + b^2 + c^2 + 2ab + 2bc + 2ca")
    print("6.  (a – b – c)^2 = a^2 + b^2 + c^2 – 2ab + 2bc – 2ca")
    print("7.  (a + b)^3     = a^3 + 3a^2 b + 3ab^2 + b^3")
    print("8.  (a – b)^3     = a^3 – 3a^2 b + 3a b^2 – b^3 = a3 – b3 – 3ab(a – b)")
    print("9.  a^3 – b^3     = (a – b)(a^2 + ab + b^2)")
    print("10. a^3 + b^3     = (a + b)(a^2 – ab + b^2)")
    print("11. (a + b)^4     = a^4 + 4a^3 b + 6a^2 b^2 + 4a b^3 + b^4")
    print("12. (a - b)^4     = a^4 - 4a^3 b + 6a^2 b^2 - 4ab^3 + b^4")
    print("13. a^4 – b^4     = (a – b)(a + b)(a^2 + b^2)")
    print("14. a^4 + b^4     = (a + b)(a – b)(a^2 – b^2)")
    print("15. a^5 – b^5     = (a – b)(a^4 + a^3 b + a^2 b^2 + ab^3 + b^4)")
    print("16. a^5 + b^5     = (a + b)(a^4 – a^3 b + a^2 b^2 – ab^3 + b^4)")
    print("17. (a - b)^5     = (a – b)(a^4 + a^3 b + a^2 b^2 + ab^3 + b^4)")
    print("18. (a + b)^5     = (a + b)(a^4 – a^3 b + a^2 b^2 – ab^3 + b^4)")
    Option = input("Enter the option (1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12 or 13): ")

    if   Option == "1":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        print((a**2 - b**2))
    elif Option == "2":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        print((a + b)**2)
    elif Option == "3":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        print((a**2 + b**2))
    elif Option == "4":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        print((a - b)**2)
    elif Option == "5":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        c = int(input("Enter the value of c: "))
        print((a + b + c)**2)
    elif Option == "6":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        c = int(input("Enter the value of c: "))
        print((a - b - c)**2)
    elif Option == "7":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        print((a + b)**3)
    elif Option == "8":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        print((a - b)**3)
    elif Option == "9":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        print((a**3 - b**3))
    elif Option == "10":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        print((a**3 + b**3))
    elif Option == "11":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        print((a + b)**4)
    elif Option == "12":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        print((a - b)**4)
    elif Option == "13":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        print((a**4 - b**4))
    elif Option == "14":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        print((a**4 + b**4))
    elif Option == "15":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        print((a**5 - b**5))
    elif Option == "16":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        print((a**5 + b**5))
    elif Option == "17":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        print((a - b)**5)
    elif Option == "18":
        a = int(input("Enter the value of a: "))
        b = int(input("Enter the value of b: "))
        print((a + b)**5)
    else:
        print("Invalid option. Please enter a number between 1 and 18.")

def convert():
    print("Choose a convert option:")
    print("1. Area")
    print("2. Volume")
    print("3. Mass")
    print("4. Time")
    print("5. Speed")
    print("6. Temperature")
    print("7. Pressure")
    print("8. Energy")
    print("9. Power")
    print("10. Electricity")
    print("11. Force")
    print("12. Angle")
    print("13. Currency")
    print("14. Length")
    print("15. Digital Storage")
    print("16. Weight")
    print("17. Heat")
    print("18. Pressure")

    option = input("Enter the option (1 to 18): ")

    if option == "1":
        area_conversion()
    elif option == "2":
        volume_conversion()
    elif option == "3":
        mass_conversion()
    elif option == "4":
        time_conversion()
    elif option == "5":
        speed_conversion()
    elif option == "6":
        temperature_conversion()
    elif option == "7":
        pressure_conversion()
    elif option == "8":
        energy_conversion()
    elif option == "9":
        power_conversion()
    elif option == "10":
        electricity_conversion()
    elif option == "11":
        force_conversion()
    elif option == "12":
        angle_conversion()
    elif option == "13":
        currency_conversion()
    elif option == "14":
        length_conversion()
    elif option == "15":
        digital_storage_conversion()
    elif option == "16":
        weight_conversion()
    elif option == "17":
        heat_conversion()
    elif option == "18":
        pressure_conversion()
    else:
        print("Invalid option. Please enter a number between 1 and 18.")


def speed_conversion():
    print("Choose a speed unit to convert:")
    print("1. Meters per Second")
    print("2. Kilometers per Hour")
    print("3. Miles per Hour")
    print("4. Knots")
    print("5. Feet per Second")

    from_unit = int(input("Enter the option for the source speed unit (1 to 5): "))
    to_unit = int(input("Enter the option for the target speed unit (1 to 5): "))

    if from_unit == to_unit:
        print("Source and target units are the same. No conversion needed.")
        return

    speed_value = float(input("Enter the speed value: "))

    conversion_factors = [
        1,         # Meters per Second to Meters per Second
        0.277778,  # Kilometers per Hour to Meters per Second
        0.44704,   # Miles per Hour to Meters per Second
        0.514444,  # Knots to Meters per Second
        0.3048     # Feet per Second to Meters per Second
    ]

    converted_value = speed_value * conversion_factors[from_unit - 1] / conversion_factors[to_unit - 1]

    print(f"{speed_value} speed unit(s) in unit {from_unit} is equal to {converted_value} speed unit(s) in unit {to_unit}.")

def temperature_conversion():
    print("Choose a temperature scale to convert:")
    print("1. Celsius")
    print("2. Fahrenheit")
    print("3. Kelvin")

    from_scale = int(input("Enter the option for the source temperature scale (1 to 3): "))
    to_scale = int(input("Enter the option for the target temperature scale (1 to 3): "))

    if from_scale == to_scale:
        print("Source and target scales are the same. No conversion needed.")
        return

    temperature_value = float(input("Enter the temperature value: "))

    conversion_functions = [
        fahrenheit_to_celsius,  # Fahrenheit to Celsius
        kelvin_to_celsius,  # Kelvin to Celsius
        celsius_to_fahrenheit,  # Celsius to Fahrenheit
        kelvin_to_fahrenheit,  # Kelvin to Fahrenheit
        celsius_to_kelvin,  # Celsius to Kelvin
        fahrenheit_to_kelvin,  # Fahrenheit to Kelvin
    ]

    # Conversion factors
    celsius_to_kelvin = lambda c: c + 273.15
    kelvin_to_celsius = lambda k: k - 273.15
    celsius_to_fahrenheit = lambda c: c * 9/5 + 32
    fahrenheit_to_celsius = lambda f: (f - 32) * 5/9
    fahrenheit_to_kelvin = lambda f: celsius_to_kelvin(fahrenheit_to_celsius(f))
    kelvin_to_fahrenheit = lambda k: celsius_to_fahrenheit(kelvin_to_celsius(k))



    converted_value = conversion_functions[(from_scale - 1) * 3 + (to_scale - 1)](temperature_value)

    print(f"{temperature_value} degrees in scale {from_scale} is equal to {converted_value} degrees in scale {to_scale}.")


def pressure_conversion():
    print("Choose a pressure unit to convert:")
    print("1. Pascal")
    print("2. Kilopascal")
    print("3. Megapascal")
    print("4. Bar")
    print("5. Millimeter of Mercury")
    print("6. Inch of Mercury")
    print("7. Pound per Square Inch (psi)")

    from_unit = int(input("Enter the option for the source pressure unit (1 to 7): "))
    to_unit = int(input("Enter the option for the target pressure unit (1 to 7): "))

    if from_unit == to_unit:
        print("Source and target units are the same. No conversion needed.")
        return

    pressure_value = float(input("Enter the pressure value: "))

    conversion_factors = [
        1,            # Pascal to Pascal
        0.001,        # Kilopascal to Pascal
        1e-6,         # Megapascal to Pascal
        1e-5,         # Bar to Pascal
        133.322,      # Millimeter of Mercury to Pascal
        3386.39,      # Inch of Mercury to Pascal
        6894.76       # Pound per Square Inch (psi) to Pascal
    ]

    converted_value = pressure_value * conversion_factors[from_unit - 1] / conversion_factors[to_unit - 1]

    print(f"{pressure_value} pressure unit(s) in unit {from_unit} is equal to {converted_value} pressure unit(s) in unit {to_unit}.")

def energy_conversion():
    print("Choose an energy unit to convert:")
    print("1. Joule")
    print("2. Kilojoule")
    print("3. Calorie")
    print("4. Kilocalorie")
    print("5. Watt-hour")
    print("6. Kilowatt-hour")
    print("7. Electronvolt")
    print("8. British Thermal Unit (BTU)")
    print("9. Foot-pound")

    from_unit = int(input("Enter the option for the source energy unit (1 to 9): "))
    to_unit = int(input("Enter the option for the target energy unit (1 to 9): "))

    if from_unit == to_unit:
        print("Source and target units are the same. No conversion needed.")
        return

    energy_value = float(input("Enter the energy value: "))

    conversion_factors = [
        1,                    # Joule to Joule
        0.001,                # Kilojoule to Joule
        4.184,                # Calorie to Joule
        4184,                 # Kilocalorie to Joule
        3600,                 # Watt-hour to Joule
        3.6e6,                # Kilowatt-hour to Joule
        1.60218e-19,          # Electronvolt to Joule
        1055.06,              # BTU to Joule
        1.35582,              # Foot-pound to Joule
    ]

    converted_value = energy_value * conversion_factors[from_unit - 1] / conversion_factors[to_unit - 1]

    print(f"{energy_value} energy unit(s) in unit {from_unit} is equal to {converted_value} energy unit(s) in unit {to_unit}.")


def power_conversion():
    print("Choose a power unit to convert:")
    print("1. Watt")
    print("2. Kilowatt")
    print("3. Megawatt")
    print("4. Gigawatt")
    print("5. Horsepower")

    from_unit = int(input("Enter the option for the source power unit (1 to 5): "))
    to_unit = int(input("Enter the option for the target power unit (1 to 5): "))

    if from_unit == to_unit:
        print("Source and target units are the same. No conversion needed.")
        return

    power_value = float(input("Enter the power value: "))

    conversion_factors = [
        1,              # Watt to Watt
        0.001,          # Kilowatt to Watt
        1e-6,           # Megawatt to Watt
        1e-9,           # Gigawatt to Watt
        0.00134102      # Horsepower to Watt
    ]

    converted_value = power_value * conversion_factors[from_unit - 1] / conversion_factors[to_unit - 1]

    print(f"{power_value} power unit(s) in unit {from_unit} is equal to {converted_value} power unit(s) in unit {to_unit}.")

def electricity_conversion():
    print("Choose an electricity unit to convert:")
    print("1. Ampere")
    print("2. Volt")
    print("3. Ohm")
    print("4. Watt")
    print("5. Joule per second (Watt)")
    print("6. Coulomb")
    print("7. Volt-Ampere (VA)")
    print("8. Farad")
    print("9. Henry")

    from_unit = int(input("Enter the option for the source electricity unit (1 to 9): "))
    to_unit = int(input("Enter the option for the target electricity unit (1 to 9): "))

    if from_unit == to_unit:
        print("Source and target units are the same. No conversion needed.")
        return

    value = float(input("Enter the electricity value: "))

    conversion_factors = [
        1,              # Ampere to Ampere (no conversion)
        2,              # Ampere to Volt
        0.5,            # Ampere to Ohm
        10,             # Ampere to Watt
        10,             # Ampere to Joule per second (Watt)
        0.001,          # Ampere to Coulomb
        1,              # Ampere to Volt-Ampere (VA)
        0.01,           # Ampere to Farad
        0.001           # Ampere to Henry
    ]

    converted_value = value * conversion_factors[from_unit - 1] / conversion_factors[to_unit - 1]

    print(f"{value} electricity unit(s) in unit {from_unit} is equal to {converted_value} electricity unit(s) in unit {to_unit}.")

def force_conversion():
    print("Choose a force unit to convert:")
    print("1. Newton")
    print("2. Kilonewton")
    print("3. Dyne")
    print("4. Pound-force")
    print("5. Kilogram-force")

    from_unit = int(input("Enter the option for the source force unit (1 to 5): "))
    to_unit = int(input("Enter the option for the target force unit (1 to 5): "))

    if from_unit == to_unit:
        print("Source and target units are the same. No conversion needed.")
        return

    force_value = float(input("Enter the force value: "))

    conversion_factors = [
        1,                # Newton to Newton
        0.001,            # Kilonewton to Newton
        100000,           # Dyne to Newton
        4.44822,          # Pound-force to Newton
        9.80665           # Kilogram-force to Newton
    ]

    converted_value = force_value * conversion_factors[from_unit - 1] / conversion_factors[to_unit - 1]

    print(f"{force_value} force unit(s) in unit {from_unit} is equal to {converted_value} force unit(s) in unit {to_unit}.")

def angle_conversion():
    print("Choose an angle unit to convert:")
    print("1. Degree")
    print("2. Radian")
    print("3. Gradian")

    from_unit = int(input("Enter the option for the source angle unit (1 to 3): "))
    to_unit = int(input("Enter the option for the target angle unit (1 to 3): "))

    if from_unit == to_unit:
        print("Source and target units are the same. No conversion needed.")
        return

    angle_value = float(input("Enter the angle value: "))

    conversion_factors = [
        1,                     # Degree to Degree
        0.0174533,             # Radian to Degree
        1.11111                # Gradian to Degree
    ]

    converted_value = angle_value * conversion_factors[from_unit - 1] / conversion_factors[to_unit - 1]

    print(f"{angle_value} angle unit(s) in unit {from_unit} is equal to {converted_value} angle unit(s) in unit {to_unit}.")

def currency_conversion():
    print("Choose a currency to convert:")
    print("1. US Dollar (USD)")
    print("2. Euro (EUR)")
    print("3. British Pound (GBP)")
    print("4. Australian Dollar (AUD)")
    print("5. Myanmar Kyat (MMK)")

    from_currency = int(input("Enter the option for the source currency (1 to 5): "))
    to_currency = int(input("Enter the option for the target currency (1 to 5): "))

    if from_currency == to_currency:
        print("Source and target currencies are the same. No conversion needed.")
        return

    amount = float(input("Enter the amount in the source currency: "))

    # Fixed conversion rates for demonstration purposes
    conversion_rates = {
        (1, 2): 0.85,    # USD to EUR
        (1, 3): 0.73,    # USD to GBP
        (1, 4): 1.34,    # USD to AUD
        (1, 5): 1539.25, # USD to MMK
        (2, 1): 1.18,    # EUR to USD
        (2, 3): 0.87,    # EUR to GBP
        (2, 4): 1.55,    # EUR to AUD
        (2, 5): 1797.35, # EUR to MMK
        # Add more conversion rates later
    }

    if (from_currency, to_currency) in conversion_rates:
        conversion_rate = conversion_rates[(from_currency, to_currency)]
        converted_amount = amount * conversion_rate
        print(f"{amount} {currency_name(from_currency)} is equal to {converted_amount} {currency_name(to_currency)}.")
    else:
        print("Invalid currency options.")

def currency_name(currency_code):
    currency_names = {
        1: "US Dollar",
        2: "Euro",
        3: "British Pound",
        4: "Australian Dollar",
        5: "Myanmar Kyat"
        # Add more currency names as needed, will add more later
    }
    return currency_names.get(currency_code, "Unknown Currency")

def length_conversion():
    print("Choose a length unit to convert:")
    print("1. Meter (m)")
    print("2. Kilometer (km)")
    print("3. Mile (mi)")
    print("4. Yard (yd)")
    print("5. Foot (ft)")
    print("6. Inch (in)")
    print("7. Hectare (ha)")
    print("8. Acre (ac)")

    from_unit = int(input("Enter the option for the source length unit (1 to 8): "))
    to_unit = int(input("Enter the option for the target length unit (1 to 8): "))

    if from_unit == to_unit:
        print("Source and target units are the same. No conversion needed.")
        return

    length_value = float(input("Enter the length value: "))

    conversion_factors = [
        1,              # Meter to Meter
        0.001,          # Kilometer to Meter
        1609.34,        # Mile to Meter
        0.9144,         # Yard to Meter
        0.3048,         # Foot to Meter
        0.0254,         # Inch to Meter
        10000,          # Hectare to Square Meter
        4046.86         # Acre to Square Meter
    ]

    converted_value = length_value * conversion_factors[from_unit - 1] / conversion_factors[to_unit - 1]

    print(f"{length_value} length unit(s) in unit {from_unit} is equal to {converted_value} length unit(s) in unit {to_unit}.")


def digital_storage_conversion():
    print("Choose a data storage unit to convert:")
    print("1. Byte (B)")
    print("2. Kilobyte (KB)")
    print("3. Megabyte (MB)")
    print("4. Gigabyte (GB)")
    print("5. Terabyte (TB)")
    print("6. Petabyte (PB)")
    print("7. Exabyte (EB)")
    print("8. Zettabyte (ZB)")
    print("9. Yottabyte (YB)")
    print("10. Bit (bit)")
    print("11. Kilobit (Kb)")
    print("12. Megabit (Mb)")
    print("13. Gigabit (Gb)")
    print("14. Terabit (Tb)")
    print("15. Petabit (Pb)")
    print("16. Exabit (Eb)")
    print("17. Zettabit (Zb)")

    from_unit = int(input("Enter the option for the source data storage unit (1 to 17): "))
    to_unit = int(input("Enter the option for the target data storage unit (1 to 17): "))

    if from_unit == to_unit:
        print("Source and target units are the same. No conversion needed.")
        return

    data_value = float(input("Enter the data value: "))

    conversion_factors = [
        1,               # Byte to Byte
        0.001,           # Kilobyte to Byte
        0.000001,        # Megabyte to Byte
        1e-9,            # Gigabyte to Byte
        1e-12,           # Terabyte to Byte
        1e-15,           # Petabyte to Byte
        1e-18,           # Exabyte to Byte
        1e-21,           # Zettabyte to Byte
        1e-24,           # Yottabyte to Byte
        8,               # Bit to Byte
        0.001,           # Kilobit to Byte
        1e-6,            # Megabit to Byte
        1e-9,            # Gigabit to Byte
        1e-12,           # Terabit to Byte
        1e-15,           # Petabit to Byte
        1e-18,           # Exabit to Byte
        1e-21            # Zettabit to Byte
    ]

    converted_value = data_value * conversion_factors[from_unit - 1] / conversion_factors[to_unit - 1]

    print(f"{data_value} data storage unit(s) in unit {from_unit} is equal to {converted_value} data storage unit(s) in unit {to_unit}.")


def weight_conversion():
    print("Choose a weight unit to convert:")
    print("1. Kilogram (kg)")
    print("2. Gram (g)")
    print("3. Milligram (mg)")
    print("4. Microgram (μg)")
    print("5. Imperial Ton (ton)")
    print("6. US Ton (ton)")
    print("7. Stone (st)")
    print("8. Pound (lb)")
    print("9. Ounce (oz)")

    from_unit = int(input("Enter the option for the source weight unit (1 to 9): "))
    to_unit = int(input("Enter the option for the target weight unit (1 to 9): "))

    if from_unit == to_unit:
        print("Source and target units are the same. No conversion needed.")
        return

    weight_value = float(input("Enter the weight value: "))

    conversion_factors = [
        1,                   # Kilogram to Kilogram
        1000,                # Gram to Kilogram
        1e6,                 # Milligram to Kilogram
        1e9,                 # Microgram to Kilogram
        0.000984207,         # Imperial Ton to Kilogram
        0.00110231,          # US Ton to Kilogram
        6.35029,             # Stone to Kilogram
        0.453592,            # Pound to Kilogram
        0.0283495            # Ounce to Kilogram
    ]

    converted_value = weight_value * conversion_factors[from_unit - 1] / conversion_factors[to_unit - 1]

    print(f"{weight_value} weight unit(s) in unit {from_unit} is equal to {converted_value} weight unit(s) in unit {to_unit}.")

def heat_conversion():
    print("Choose a heat unit to convert:")
    print("1. Joule (J)")
    print("2. Kilojoule (kJ)")
    print("3. Calorie (cal)")
    print("4. Kilocalorie (kcal)")
    print("5. British Thermal Unit (BTU)")
    print("6. Foot-Pound (ft-lb)")

    from_unit = int(input("Enter the option for the source heat unit (1 to 6): "))
    to_unit = int(input("Enter the option for the target heat unit (1 to 6): "))

    if from_unit == to_unit:
        print("Source and target units are the same. No conversion needed.")
        return

    heat_value = float(input("Enter the heat value: "))

    conversion_factors = [
        1,               # Joule to Joule
        0.001,           # Kilojoule to Joule
        0.239006,        # Calorie to Joule
        239.006,         # Kilocalorie to Joule
        0.000947817,     # BTU to Joule
        1.35582          # Foot-Pound to Joule
    ]

    converted_value = heat_value * conversion_factors[from_unit - 1] / conversion_factors[to_unit - 1]

    print(f"{heat_value} heat unit(s) in unit {from_unit} is equal to {converted_value} heat unit(s) in unit {to_unit}.")

def pressure_conversion():
    print("Choose a pressure unit to convert:")
    print("1. Pascal (Pa)")
    print("2. Kilopascal (kPa)")
    print("3. Megapascal (MPa)")
    print("4. Bar (bar)")
    print("5. Millimeter of Mercury (mmHg)")
    print("6. Pound per Square Inch (psi)")

    from_unit = int(input("Enter the option for the source pressure unit (1 to 6): "))
    to_unit = int(input("Enter the option for the target pressure unit (1 to 6): "))

    if from_unit == to_unit:
        print("Source and target units are the same. No conversion needed.")
        return

    pressure_value = float(input("Enter the pressure value: "))

    conversion_factors = [
        1,               # Pascal to Pascal
        0.001,           # Kilopascal to Pascal
        1e-6,            # Megapascal to Pascal
        1e-5,            # Bar to Pascal
        7.50062,         # Millimeter of Mercury to Pascal
        6894.76          # Pound per Square Inch to Pascal
    ]

    converted_value = pressure_value * conversion_factors[from_unit - 1] / conversion_factors[to_unit - 1]

    print(f"{pressure_value} pressure unit(s) in unit {from_unit} is equal to {converted_value} pressure unit(s) in unit {to_unit}.")

# Placeholder functions for different conversions
def area_conversion():
    print("Choose an area unit to convert:")
    print("1. Square Kilometer")
    print("2. Square Meter")
    print("3. Square Mile")
    print("4. Square Yard")
    print("5. Square Foot")
    print("6. Square Inch")
    print("7. Hectare")
    print("8. Acre")

    from_unit = int(input("Enter the option for the source area unit (1 to 8): "))
    to_unit = int(input("Enter the option for the target area unit (1 to 8): "))

    if from_unit == to_unit:
        print("Source and target units are the same. No conversion needed.")
        return

    area_value = float(input("Enter the area value: "))

    conversion_factors = [
        1e-6,   # Square Kilometer to Square Meter
        1,      # Square Meter to Square Meter
        2.58999e6,  # Square Mile to Square Meter
        1.19599, # Square Yard to Square Meter
        10.7639, # Square Foot to Square Meter
        1550.0031, # Square Inch to Square Meter
        1e4,    # Hectare to Square Meter
        4046.86 # Acre to Square Meter
    ]

    converted_value = area_value * conversion_factors[from_unit - 1] / conversion_factors[to_unit - 1]

    print(f"{area_value} square unit(s) in unit {from_unit} is equal to {converted_value} square unit(s) in unit {to_unit}.")



def volume_conversion():
    print("Choose a volume unit to convert:")
    print("1. US Liquid Gallon")
    print("2. US Liquid Quart")
    print("3. US Liquid Pint")
    print("4. US Legal Cup")
    print("5. US Fluid Ounce")
    print("6. US Tablespoon")
    print("7. US Teaspoon")
    print("8. Cubic Meter")
    print("9. Liter")
    print("10. Milliliter")
    print("11. Imperial Gallon")
    print("12. Imperial Quart")
    print("13. Imperial Pint")
    print("14. Imperial Cup")
    print("15. Imperial Fluid Ounce")
    print("16. Imperial Tablespoon")
    print("17. Imperial Teaspoon")
    print("18. Cubic Foot")
    print("19. Cubic Inch")

    from_unit = int(input("Enter the option for the source volume unit (1 to 19): "))
    to_unit = int(input("Enter the option for the target volume unit (1 to 19): "))

    if from_unit == to_unit:
        print("Source and target units are the same. No conversion needed.")
        return

    volume_value = float(input("Enter the volume value: "))

    conversion_factors = [
        3.78541,    # US Liquid Gallon to Liter
        0.946353,   # US Liquid Quart to Liter
        0.473176,   # US Liquid Pint to Liter
        0.24,       # US Legal Cup to Liter
        0.0295735,  # US Fluid Ounce to Liter
        0.01479,    # US Tablespoon to Liter
        0.00492892, # US Teaspoon to Liter
        1000,       # Cubic Meter to Liter
        1,          # Liter to Liter
        0.001,      # Milliliter to Liter
        4.54609,    # Imperial Gallon to Liter
        1.13652,    # Imperial Quart to Liter
        0.568261,   # Imperial Pint to Liter
        0.284131,   # Imperial Cup to Liter
        0.0284131,  # Imperial Fluid Ounce to Liter
        0.0177582,  # Imperial Tablespoon to Liter
        0.00591939, # Imperial Teaspoon to Liter
        28.3168,    # Cubic Foot to Liter
        0.0163871   # Cubic Inch to Liter
    ]

    converted_value = volume_value * conversion_factors[from_unit - 1] / conversion_factors[to_unit - 1]

    print(f"{volume_value} volume unit(s) in unit {from_unit} is equal to {converted_value} volume unit(s) in unit {to_unit}.")



def mass_conversion():
    print("Choose a mass unit to convert:")
    print("1. Tonne")
    print("2. Kilogram")
    print("3. Gram")
    print("4. Milligram")
    print("5. Microgram")
    print("6. Imperial Ton")
    print("7. US Ton")
    print("8. Stone")
    print("9. Pound")
    print("10. Ounce")

    from_unit = int(input("Enter the option for the source mass unit (1 to 10): "))
    to_unit = int(input("Enter the option for the target mass unit (1 to 10): "))

    if from_unit == to_unit:
        print("Source and target units are the same. No conversion needed.")
        return

    mass_value = float(input("Enter the mass value: "))

    conversion_factors = [
        1000,        # Tonne to Kilogram
        1,           # Kilogram to Kilogram
        0.001,       # Gram to Kilogram
        1e-6,        # Milligram to Kilogram
        1e-9,        # Microgram to Kilogram
        1016.05,     # Imperial Ton to Kilogram
        907.185,     # US Ton to Kilogram
        6.35029,     # Stone to Kilogram
        0.453592,    # Pound to Kilogram
        0.0283495    # Ounce to Kilogram
    ]

    converted_value = mass_value * conversion_factors[from_unit - 1] / conversion_factors[to_unit - 1]

    print(f"{mass_value} mass unit(s) in unit {from_unit} is equal to {converted_value} mass unit(s) in unit {to_unit}.")


def time_conversion():
    print("Choose a time unit to convert:")
    print("1. Nanosecond")
    print("2. Microsecond")
    print("3. Millisecond")
    print("4. Second")
    print("5. Minute")
    print("6. Hour")
    print("7. Day")
    print("8. Week")
    print("9. Month")
    print("10. Calendar Year")
    print("11. Year")
    print("12. Decade")
    print("13. Century")

    from_unit = int(input("Enter the option for the source time unit (1 to 13): "))
    to_unit = int(input("Enter the option for the target time unit (1 to 13): "))

    if from_unit == to_unit:
        print("Source and target units are the same. No conversion needed.")
        return

    time_value = float(input("Enter the time value: "))

    conversion_factors = [
        1e-9,      # Nanosecond to Second
        1e-6,      # Microsecond to Second
        0.001,     # Millisecond to Second
        1,         # Second to Second
        60,        # Minute to Second
        3600,      # Hour to Second
        86400,     # Day to Second
        604800,    # Week to Second
        2.63e+6,   # Month to Second (Assuming an average month of 30.44 days)
        3.154e+7,  # Calendar Year to Second (Assuming an average year of 365.25 days)
        3.154e+7,  # Year to Second (Assuming an average year of 365.25 days)
        3.154e+8,  # Decade to Second
        3.154e+9   # Century to Second
    ]

    converted_value = time_value * conversion_factors[from_unit - 1] / conversion_factors[to_unit - 1]

    print(f"{time_value} time unit(s) in unit {from_unit} is equal to {converted_value} time unit(s) in unit {to_unit}.")

# Function to handle the calculator command and options
def calculator():
    print("Choose a calculator option:")
    print("1. Simple Calculator")
    print("2. Complex Calculator")

    option = input("Enter the option (1 or 2): ")

    if option == "1":
        simple_calculator()
    elif option == "2":
        complex_calculator()
    else:
        print("Invalid option. Please enter 1 or 2.")

def check_feedback_file(directory):
    feedback_file_path = os.path.join(directory, "feedback.txt")
    if not os.path.exists(feedback_file_path):
        with open(feedback_file_path, "w") as feedback_file:
            feedback_file.write("")

def feedback():
    feedback_text = input("How can we improve? ")

    # Directory containing the feedback file
    feedback_directory = r"D:\oosey\sayar wai hw and class work\free time code\visual studio\commands\feedback"

    # Check if feedback file exists, if not, create one
    check_feedback_file(feedback_directory)

    # Path to the feedback file
    feedback_file_path = os.path.join(feedback_directory, "feedback.txt")

    # Write feedback to the file
    with open(feedback_file_path, "a") as feedback_file:
        feedback_file.write(feedback_text + "\n")

    print("Thank you for your feedback!")

def check_feedback_file(directory):
    feedback_file_path = os.path.join(directory, "feedback.txt")
    if not os.path.exists(feedback_file_path):
        with open(feedback_file_path, "w") as feedback_file:
            feedback_file.write("Feedback:\n")

def read_file():
    feedback_directory = r"D:\oosey\sayar wai hw and class work\free time code\visual studio\commands\feedback"
    feedback_file_path = os.path.join(feedback_directory, "feedback.txt")
    
    # Check if feedback file exists
    if not os.path.exists(feedback_file_path):
        print("Feedback file does not exist.")
        return

    # Prompt user for password
    password = input("Enter the password to read feedback: ")

    # Hardcoded admin password
    admin_password = "Iloveonepiece1!"

    if password != admin_password:
        print("Incorrect password. Access denied.")
        return

    # Read and print the contents of the feedback file
    with open(feedback_file_path, "r") as feedback_file:
        feedback_content = feedback_file.read()
        print("\n" + feedback_content)


# Function to exit the program
def exit_program(name):
    print(f"Goodbye, {name}.")

# Ask for the user's name
name = input("Enter your name: ")

print(f"Hello, user {name}! What can I help you with?")

while True:
    user_input = input("Enter a command: ")

    if "time" in user_input.lower():
        display_time()
        
    elif "date" in user_input.lower() or "day" in user_input.lower():
        display_date()

    elif "remind" in user_input.lower():
        remind()

    elif "youtube" in user_input.lower():
        video_name = input("Sure, what would you like me to search for on YouTube? ")
        search_youtube(video_name)

    elif "web" in user_input.lower():
        search_engines = {
            "1": "Google",
            "2": "Bing",
            "3": "Yahoo",
            "4": "Baidu",
            "5": "Yandex",
            "6": "Ask.com",
            "7": "AOL Search",
            "8": "Ecosia",
            "9": "Search.com"
        }

        print("These are the search engines you can choose from:")
        for number, engine in search_engines.items():
            print(f"{number}. {engine}")

        user_choice = input("Enter the number or name of the search engine: ")
        if user_choice in search_engines:
            selected_engine = search_engines[user_choice]
        elif user_choice.isdigit() and int(user_choice) <= len(search_engines):
            selected_engine = list(search_engines.values())[int(user_choice) - 1]
        else:
            print("Invalid search engine choice.")
            continue  # Continue to the next iteration of the while loop

        keyword = input(f"What would you like to search for on {selected_engine}? ")
        search_web(keyword, selected_engine)

    elif "search" in user_input.lower():
        file_name_to_search = input("Enter the name of the file to search for: ")
        search_files(file_name_to_search)
    elif "delete" in user_input.lower():
        delete_choice = input("Do you want to delete a file or a folder? Enter 'file' or 'folder': ").lower()
        if delete_choice == "file":
            delete_file_dialog()
        elif delete_choice == "folder":
            delete_folder_dialog()
        else:
            print("Invalid choice. Please enter 'file' or 'folder'.")

    elif "weather" in user_input.lower():
        city = input("Enter the city for weather information: ")
        api_key = "09d5fe82d05e6d3cb705353dba5c98cd"  # Replace with your actual API key
        get_weather(api_key, city)

    elif "joke" in user_input.lower() or "humor" in user_input.lower():
        get_joke()
    elif "fact" in user_input.lower():
        get_fact()

    elif "ascii" in user_input.lower():
        text_to_convert = input("Enter the text you want to convert to ASCII art: ")

        # Display available fonts
        print("Available Fonts:")
        for font in available_fonts:
            print(font)

        # Choose a font
        font_choice = input("Choose a font (e.g., 'block'): ")

        if font_choice.lower() in available_fonts:
            ascii_art = get_ascii_art(text_to_convert, font_choice)
            print(ascii_art)
        else:
            print("Invalid font choice. Please choose from the available fonts.")
    elif "game" in user_input.lower():
        print("Available Games:")
        print("1. Rock, Paper, Scissors")
        print("2. Tic Tac Toe")

        game_choice = input("Enter the number of the game you want to play (or 'exit' to go back): ")

        if game_choice == '1':
            rock_paper_scissors_game()
        elif game_choice == '2':
            tic_tac_toe_game()
        elif game_choice.lower() == 'exit':
            print("Exiting game selection.")
        else:
            print("Invalid game choice. Please enter a valid number or 'exit'.")
    elif "calculator" in user_input.lower():
        calculator()

    elif "feedback" in user_input.lower():
        feedback()

    elif "read" in user_input.lower():
        read_file()

    elif "tkinter" in user_input.lower():
        create_tkinter_window()

    elif "help" in user_input.lower():
        # Display available commands when the user enters "help"
        print("Below are the current commands I can execute as of right now, " + name)
        print("- 'time'                 :   Display the current time.")
        print("- 'date'                 :   Display the current date in dd/mm/yyyy format.")
        print("- 'remind'               :   Remember a task.")
        print("- 'youtube'              :   Search for videos on YouTube.")
        print("- 'web'                  :   Browse the web using various search engines.")
        print("- 'search'               :   Search for files on all drives.")
        print("- 'delete'               :   Delete files or folder")
        print("- 'weather'              :   Display weather")
        print("- 'joke'                 :   Display a random joke")
        print("_ 'fact'                 :   Display a random fact")
        print("- 'ascii'                :   Display ascii art")
        print("- 'game'                 :   Display available games and play a game.")
        print("- 'calculator'           :   Caluculate matfhs")
        print("- 'feedback'             :   Give feedback")
        print("- 'read'                 :   Read the feedback file *Admin only* ")
        print("- 'exit', 'quit', 'close':   Exit the program.")
    
    
    elif user_input.lower() in ["exit", "quit", "close"]:
        exit_program(name)
        quit()
    
    else:
        # If not a recognized command, treat it as a regular sentence
        if not user_input.lower().startswith("can you"):
            print("You entered:", user_input)
            print("If you don't know what commands there are, type help.")
