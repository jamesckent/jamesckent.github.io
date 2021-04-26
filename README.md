# Code for Agent-based modelling university practical
"""
Created on Fri Mar 19 12:08:07 2021

@author: james
"""

#---------Imports
import random
import operator
import agentframework
import csv
import tkinter
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg
from matplotlib.figure import Figure
#import matplotlib
#matplotlib.use('TkAgg')
import matplotlib.pyplot
import matplotlib.animation
#---------End of imports


num_of_agents = 10
num_of_iterations = 10
neighbourhood = 20


# Combining code with CSV reader to create the environment
f = open('in.txt', newline='')
reader = csv.reader(f,  quoting=csv.QUOTE_NONNUMERIC)


# Environment list
environment = []
for row in reader:
    rowlist = []
    for value in row:
        rowlist.append(value)
    environment.append(rowlist)


# Agents list
agents = []

random.shuffle(agents) # Randomise order in which agents are processed 

fig = matplotlib.pyplot.figure(figsize=(7, 7))
ax = fig.add_axes([0, 0, 1, 1])
#ax.set_autoscale_on(False)

# Making the agents
for i in range(num_of_agents):
    agents.append(agentframework.Agent(environment, agents))

#carry_on = True	

def update(frame_number):
    
    fig.clear()
    #global carry_on

# Moving the agents
    for j in range(num_of_iterations):
        for i in range(num_of_agents):
            agents[i].move()
            agents[i].eat()
            agents[i].share_with_neighbours(neighbourhood)
        
        #if random.random() < 0.1:
            #carry_on = False
            #print("stopping condition")

# Plot agent locations
        matplotlib.pyplot.xlim(0, 99)
        matplotlib.pyplot.ylim(0, 99) 
        matplotlib.pyplot.imshow(environment)
        for i in range(num_of_agents):
            matplotlib.pyplot.scatter(agents[i].x, agents[i].y)


# Implementing a generator function as part of stopping condiiton
#def gen_function(b = [0]):
    #a = 0
    #global carry_on
    #while (a < 10) & (carry_on) :
        #yield a
        #a = a + 1


# Animate agents
#animation = matplotlib.animation.FuncAnimation(fig, update, interval=1, repeat=False, frames=num_of_iterations)
#animation = matplotlib.animation.FuncAnimation(fig, update, frames=gen_function, repeat=False)


#matplotlib.pyplot.show()


# Creating GUI
def run():
    animation = matplotlib.animation.FuncAnimation(fig, update, interval=1, repeat=False, frames=num_of_iterations)
    canvas.draw()

root = tkinter.Tk() # Main window

# Filling window with objects
root.wm_title("Model")
canvas = matplotlib.backends.backend_tkagg.FigureCanvasTkAgg(fig, master=root)
canvas._tkcanvas.pack(side=tkinter.TOP, fill=tkinter.BOTH, expand=1)
menu_bar = tkinter.Menu(root)
root.config(menu=menu_bar)
model_menu = tkinter.Menu(menu_bar)
menu_bar.add_cascade(label="Model", menu=model_menu)
model_menu.add_command(label="Run model", command=run)

tkinter.mainloop() # Wait for interactions.
