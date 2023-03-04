# RishuVA
Virtual Assistant 
import calendar
import subprocess
import pyttsx3
import speech_recognition as sr
import datetime
import wikipedia
import webbrowser
import os
import pyjokes
import smtplib
import ctypes
import time
import cv2


engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[1].id)

def speak(audio):
	engine.say(audio)
	engine.runAndWait()

def wishMe():
	hour = int(datetime.datetime.now().hour)
	if hour >= 0 and hour < 12:
		speak("Good Morning Sir !")

	elif hour >= 12 and hour < 18:
		speak("Good Afternoon  !")

	else:
		speak("Good Evening Sir !")
	assname =("My name is ZARVIS.")
	speak(assname)
	speak("I am your Personal Assistant. How can I help you.")
	

def takeCommand():
	
	r = sr.Recognizer()
	
	with sr.Microphone() as source:
		
		print("Listening...")
		r.pause_threshold = 1
		r.energy_threshold = 2500
		audio = r.listen(source)

	try:
		print("Recognizing...")
		query = r.recognize_google(audio, language ='en-in')
		print(f"User said: {query}\n")

	except Exception as e:
		# print(e)
		print("Unable to Recognize your voice. Say it again please...")
		return "None"
	
	return query

def sendEmail(to, content):
	server = smtplib.SMTP('smtp.gmail.com', 587)
	server.ehlo()
	server.starttls()
	server.login('rishabh4082000@gmail.com', "lyenqzhuuwnqethq")
	server.sendmail('rishabh4082000@gmail.com', to, content)
	server.close()

if __name__ == '__main__':
	clear = lambda: os.system('cls')
	clear()
	email = {'Rishabh':'exo.rishu@gmail.com','vishal':'thakurvishalbsr@gmail.com'}
	wishMe()
	
	while True:
		
		query = takeCommand().lower()
		
		if 'wikipedia' in query:
			speak('Searching Wikipedia...')
			query = query.replace("wikipedia", "")
			results = wikipedia.summary(query, sentences = 2)
			speak("According to Wikipedia")
			print(results)
			speak(results)

		elif 'open youtube' in query:
			speak("Here you go to Youtube\n")
			webbrowser.open("youtube.com")

		elif "open chrome" in query:
			speak("Here you go to chrome\n")
			gc = "C:\\Program Files\\Google\\Chrome\\Application\chrome.exe"
			os.startfile(gc)

		elif 'open google' in query:
			speak("Here you go to Google\n")
			webbrowser.open("google.com")

		elif 'open stackoverflow' in query:
			speak("Here you go to Stack Over flow. Happy coding")
			webbrowser.open("stackoverflow.com")

		elif 'play music' in query or "play song" in query:
			speak("Here you go with music")
			# music_dir = "location of the directory where your songs are preset"
			music_dir = "C:\\Users\\Hp\\Music"
			songs = os.listdir(music_dir)
			random = os.startfile(os.path.join(music_dir, songs[1]))

		elif 'the time' in query:
			strTime = datetime.datetime.now().strftime("%H:%M:%S")
			speak(f"Sir, the time is {strTime}")

		elif 'open vs code' in query:
			speak("opening Visual studio code")
			codePath ="C:\\Users\\Hp\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\Visual Studio Code\\Visual Studio Code.lnk"
			os.startfile(codePath)

		elif 'open turbo c' in query:
			speak("opening turbo c++")
			tc = "C:\\ProgramData\\Microsoft\\Windows\\Start Menu\\Programs\\Turbo C++\\Turbo C++ 3.2"
			os.startfile(tc)

		elif 'send email' in query:
			try:
				speak("What should I say?")
				content = takeCommand()
				speak("whome should i send")
				to = takeCommand().lower()
				if to in email:
				
					sendEmail(email[to], content)
					speak("Email has been sent !")
				else :
					speak("Contact not found")
			except Exception as e:
				print(e)
				speak("I am not able to send this email")

		elif 'how are you' in query:
			speak("I am fine, Thank you")
			speak("How are you, Sir")

		elif 'open camera' in query:
			cam =  cv2.VideoCapture(0)
			cv2.namedWindow("Camera")
			img_counter = 0
			speak("Press space to capture the image and press excape to Close the camera")
			while True:
				ret,frame = cam.read()
				if not ret:
					speak("failed to grab frame")
					break
				cv2.imshow("test",frame)
				k = cv2.waitKey(1)
				if k%256 == 27:
					print("Closing the camera")
					speak("Closing the camera")
					break
				elif k%256 == 32: 
					img_name = "opencv_frame_{}.png".format(img_counter)
					cv2.imwrite(img_name,frame)
					print("Image captured")
					speak("Image captured")
					img_counter += 1
			cam.release()
			cv2.destroyAllWindows()

		elif 'fine' in query or "good" in query:
			speak("It's good to know that your fine")

		elif 'exit' in query or 'bye' in query:
			speak("Thanks for giving me your time")
			exit()

		elif "who made you" in query or "who created you" in query:
			speak("I have been created by Rishabh pratap singh.")
			
		elif 'joke' in query:
			speak(pyjokes.get_joke())	

		elif 'search' in query and 'on google' in query:
			query = query.replace("search", "")
			query = query.replace("on google", "")		
			webbrowser.open(query)

		elif "who i am" in query:
			speak("If you talk then definitely your human.")

		elif "why are you being created" in query:
			speak("Thanks to Rishabh. further It's a secret")

		elif 'is love' in query:
			speak("It is 7th sense that destroy all other senses")

		elif "who are you" in query:
			speak("I am your virtual assistant created by Rishabh")

		elif 'For what purpose you are being created' in query:
			speak("I was created as a Mini project by Mr. Rishabh Singh")

		elif 'lock window' in query:
				speak("locking the device")
				ctypes.windll.user32.LockWorkStation()

		elif 'shutdown system' in query:
			speak("Hold On a Sec ! Your system is on its way to shut down")
			os.system('shutdown /s /t 1')

		elif "don't listen" in query or "stop listening" in query:
			speak("for how much time you want to stop friday from listening commands")
			a = int(takeCommand())
			time.sleep(a)
			print(a)

		elif "restart" in query:
			subprocess.call(["shutdown", "/r"])
			
		elif "hibernate" in query or "sleep" in query:
			speak("Hibernating")
			subprocess.call("shutdown /h")

		elif "log off" in query or "sign out" in query:
			speak("Make sure all the application are closed before sign-out")
			time.sleep(5)
			subprocess.call(["shutdown", "/l"])

		elif "write a note" in query:
			speak("What should i write, sir")
			note = takeCommand()
			file = open('Friday.txt', 'a')
			file.write("\n\n")
			speak("Sir, Should i include date and time")
			snfm = takeCommand()
			if 'yes' in snfm or 'sure' in snfm:
				strTime = datetime.datetime.now().strftime("%H:%M:%S")
				file.write(strTime)
				file.write(" :- ")
				file.write(note)
			else:
				file.write(note)
		
		elif "show note" in query:
			speak("Showing Notes")
			file = open("Friday.txt", "r")
			print(file.read())
			speak(file.read(6))

		
		elif 'calendar' in query:
			speak("Of which year you want to print calendar")
			y = takeCommand()
			ye = int(y)
			print (f"The calendar of year {ye} is : ")
			print (calendar.calendar(ye, 2, 1, 6))

		elif 'code' in query and 'binary search' in query:
			speak("here is your code in C language")
			file = open("binary search.txt","r")
			print(file.read())
			speak("Do You want to execute the code")
			y = takeCommand().lower()
			if 'yes' in y or 'sure' in y:
				subprocess.run('C:\\Programming\\C Language\\binarySearch')
		
		elif 'code' in query and 'fibonacci series' in query:
			speak("here is your code in C language")
			file = open("fabonic.txt","r")
			print(file.read())
			speak("Do You want to execute the code")
			y = takeCommand().lower()
			if 'yes' in y or 'sure' in y:
				subprocess.run('C:\\Programming\\C Language\\fibonacci_series')

		elif 'code' in query and 'factorial' in query:
			speak("here is your code in C language")
			file = open("factorial.txt","r")
			print(file.read())
			speak("Do You want to execute the code")
			y = takeCommand().lower()
			if 'yes' in y or 'sure' in y:
				subprocess.run('C:\\Programming\\C Language\\factorial')

		elif 'code' in query and 'palindrom' in query:
			speak("here is your code in C language")
			file = open("palindrom.txt","r")
			print(file.read())
			speak("Do You want to execute the code")
			y = takeCommand().lower()
			if 'yes' in y or 'sure' in y:
				subprocess.run('C:\\Programming\\C Language\\palindrom')

		elif 'code' in query and 'multiplication' in query and 'matrix' in query:
			speak("here is your code in C language")
			file = open("matrix mul.txt","r")
			print(file.read())
			speak("Do You want to execute the code")
			y = takeCommand().lower()
			if 'yes' in y or 'sure' in y:
				subprocess.run('C:\\Programming\\C Language\\multiplication_matrix')
		
		elif 'code' in query and 'addition' in query and 'matrix' in query:
			speak("here is your code in C language")
			file = open("matrix add.txt","r")
			print(file.read())
			speak("Do You want to execute the code")
			y = takeCommand().lower()
			if 'yes' in y or 'sure' in y:
				subprocess.run('C:\\Programming\\C Language\\addMatrix')

		elif 'open calculator' in query:
			os.system('calc.exe')

		elif 'open computer management' in query:
			os.system('CompMgmtLauncher.exe') 
		
		elif 'open control panel' in query:
			os.system('control.exe')
		
		elif 'open display colour calibration' in query:
			os.system('dccw.exe')

		elif 'open device pairing wizard' in query:
			os.system('DevicePairingWizard.exe')

		elif 'open phone dialler' in query:
			os.system('dialer.exe')
		
		elif 'open switch screen' in query:
			os.system('DisplaySwitch.exe')
		
		elif 'open display settings' in query or 'open display setting' in query:
			os.system('DpiScaling.exe')

		elif 'open private character editor' in query:
			os.system('eudcedit.exe')
		
		elif 'open event viewer' in query:
			os.system('eventvwr.exe')

		elif 'open file history' in query:
			os.system('FileHistory.exe')
		
		elif 'open bluetooth file transfer' in query or 'open bluetooth' in query:
			os.system('fsquirt.exe')
		
		elif 'open fax cover page editor' in query:
			os.system('FXSCOVER.exe')
		
		elif 'open task manager' in query:
			os.system('LaunchTM.exe')
		
		elif 'open magnifier' in query:
			os.system('Magnify.exe')
		
		elif 'open window mobility centre' in query:
			os.system('mblctr.exe')
		
		elif 'open mrt' in query:
			os.system('MRT.exe')
		
		elif 'open system configuration' in query:
			os.system('msconfig.exe')

		elif 'open microsoft support diagnostic tool' in query:
			os.system('msdt.exe')

		elif 'open system information' in query:
			os.system('msinfo32.exe')

		elif 'open windows remote assistance' in query:
			os.system('msra.exe')

		elif 'open notepad' in query:
			os.system('notepad.exe')

		elif 'open window features' in query:
			os.system('OptionalFeatures.exe')

		elif 'open on screen keyboard' in query:
			os.system('osk.exe')

		elif 'open performance Monitor' in query:
			os.system('perfmon.exe')

		elif 'open presentation settings' in query:
			os.system('PresentationSettings.exe')

		elif 'open system restore' in query:
			os.system('rstrui.exe')

		elif 'open backup and restore' in query:
			os.system('sdclt.exe')

		elif 'open user account control settings' in query:
			os.system('UserAccountControlSettings.exe')

		elif 'open work folder' in query:
			os.system('WorkFolders.exe')

		elif 'open wordpad' in query:
			os.system('write.exe')

		elif 'open file manager' in query:
			os.system('explorer.exe')

		elif 'want to play a game' in query:
			speak("which game do you want to play")
			print("1. Pacman \n2. Snake \n3. ant \n4. bangels \n5. bounce \n6. torn")
			game = takeCommand().lower()
			if 'pacman' in game:
				speak("you can play Pacman")
				exec(open('C:\\Programming\\Python\\pacman.py').read())

			elif 'snake' in game:
				speak("you can play Snake game")
				exec(open('C:\\Programming\\Python\\snake.py').read())
			
			elif 'ant' in game:
				speak("you can play ant game")
				exec(open('C:\\Programming\\Python\\ant.py').read())
			
			elif 'bagels' in game:
				speak("you can play bagels game")
				exec(open('C:\\Programming\\Python\\bagels.py').read())
			
			elif 'bounce' in game:
				speak("you can play bounce game")
				exec(open('C:\\Programming\\Python\\bounce.py').read())
				
			elif 'cannon' in game:
				speak("you can play cannon game")
				exec(open('C:\\Programming\\Python\\cannon.py').read())

			elif 'torn' in game:
				speak("you can play torn game")
				exec(open('C:\\Programming\\Python\\torn.py').read())
