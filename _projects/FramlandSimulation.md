---
title: "FarmLand Simulation"
excerpt: "
    <div style='display: flex; align-items: center; height: 20vh;'>
        <div style='flex: 0 0 70%;'>This project is a simulation of chickens and ducks interacting in a virtual environment</div>
        <img src='/images/FarmLand/farm6.jpg' style='flex: 0 0 30%; height: 100%; object-fit: contain; margin-left: 10px;'>
    </div>"
collection: Project
---

## Overview
This project is a simulation of chickens and ducks interacting in a virtual environment. It utilizes Windows Forms for the user interface and C# for the underlying logic. The simulation demonstrates the movement and behavior of virtual chickens and ducks, along with their interactions with food items.

```csharp
namespace newSimulation
{
     internal class Chicks : Animal
    {

        public Chicks Mother;
        Random random = new Random();
   
        public Chicks(int left, int right, int top, int down)
        {
            img = Properties.Resources.chick;
            posX = random.Next(left, right);
            posY = random.Next(top, down);
            height = 100;
            width = 100;
            speed = 10;
            eggPeriod = 0;
            isRoot = true;
            isMother = true;
            age = 100;
            
        }
        public Chicks(Chicks mother)
        { 
            Mother = mother;
            img = Properties.Resources.egg;
            posX = random.Next(mother.posX-5, mother.posX + 5);
            posY = random.Next(mother.posY-5, mother.posY + 5);
            height = 30;
            width = 30;
            speed = 10;
            age = 0;
            isRoot = false;
            isMother = false;
        } 
       
    }
}
```
<div>
    <br/><img src='/images/FarmLand/farm1.jpg' style="width: 100%; height: auto;">
    <br/><img src='/images/FarmLand/farm2.jpg' style="width: 100%; height: auto;">
    <br/><img src='/images/FarmLand/farm3.jpg' style="width: 100%; height: auto;">
    <br/><img src='/images/FarmLand/farm6.jpg' style="width: 100%; height: auto;">
</div>


## Technologies/Tools Used
- C# programming language
- Windows Forms for GUI
- Visual Studio for development environment

## Key Features
- Dynamic movement of chickens and ducks within the simulation window.
- Egg-laying behavior for chickens, with a growth period for eggs.
- Rotation of images to represent directional movement.
- Collision detection between food items and virtual animals.

## Challenges and Solutions
- **Dynamic Movement**: Achieved by adjusting position coordinates and utilizing timers for periodic updates.
- **Egg-Laying Mechanism**: Implemented by tracking age and growth period of eggs.
- **Image Rotation**: Utilized different images to simulate rotational movement.
- **Collision Detection**: Employed a method for detecting collisions between food and animals.

## Achievements
The project successfully creates a dynamic simulation of chickens and ducks, showcasing their movements and interactions within the virtual environment.

## Impact or Benefits
This simulation provides an engaging visual representation of chicken and duck behavior. It can be used for educational purposes or as a basis for further development of virtual animal simulations.

## Lessons Learned
- Enhanced proficiency in C# programming.
- Gained experience in modeling virtual behaviors and interactions.
- Improved skills in graphical user interface design using Windows Forms.

## Future Enhancements
Potential future improvements could include adding additional species, refining movement algorithms, and expanding the simulation environment to include more interactive elements. Additionally, optimizing performance for larger-scale simulations could be explored.



The goal of this project is to create a visually engaging simulation that mimics the movements and behaviors of chickens and ducks. The simulation includes features such as egg-laying, movement patterns, and interaction with food.

## Handler Code

```csharp
using System;
using System.CodeDom;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace newSimulation
{
    public partial class Form1 : Form
    {

        #region GLOBAL VARIABLE SECTION
        int midLine;
        int endLine;
        int bottom;
        List<Chicks> chicks = new List<Chicks>();
        List<Chicks> chicksToAdd = new List<Chicks>();
        List<Ducks> Ducks = new List<Ducks>();
        List<Ducks> DucksToAdd = new List<Ducks>();
        List<Food> Foods = new List<Food>();
        List<Food> FoodsToRemove = new List<Food>();

        #endregion
        public Form1()
        {
            InitializeComponent();
        }

        #region FORM LOAD
        private void Form1_Load(object sender, EventArgs e)
        {
            midLine = this.Width / 2;
            endLine = this.Width;
            bottom = this.Height;

            Chicks newChick = new Chicks(midLine, endLine, 50, bottom);
            chicks.Add(newChick);
         
            Ducks newDuck = new Ducks(5, midLine, Top, bottom);
            Ducks.Add(newDuck);
        }
        #endregion

        #region PAINTING

        private void Form1_Paint(object sender, PaintEventArgs e)
        {
            Graphics canvas = e.Graphics;
            /*canvas.DrawImage(chickMothers[0].img, chickMothers[0].posX, chickMothers[0].posY,
                (int)chickMothers[0].width, (int)chickMothers[0].height);
          */
            #region DRAWING DUCK
            foreach (Chicks chick in chicks)
            {

                canvas.DrawImage(chick.img, chick.posX, chick.posY,
                        (int)chick.width, (int)chick.height);
            }
            #endregion

            #region DRAWING DUCK
            foreach (Ducks duck in Ducks)
            {
                canvas.DrawImage(duck.img, duck.posX, duck.posY,
                    (int)duck.width, ( int)duck.height);
            }
            #endregion

            #region DRAWING FOOD

            foreach(Food food in Foods)
            {
                canvas.DrawImage(food.img, food.posX, food.posY,
                    (int)food.width, (int)food.hight);
            }
            
            #endregion




        }


        #endregion

        #region TIMER


        #region CHICKEN TIMER
        //TIMER OF CHICKEN
        private void chickenTimer_Tick(object sender, EventArgs e)
        {

            try
            {
                int index = 0;

                foreach (Chicks cm in chicks)
                {

                    if (!isEgg(cm.age))
                    {
                        cm.MoveAnimal();
                        cm.posX += cm.speedX;
                        cm.posY += cm.speedY;
                        cm.eggPeriod += 1;
                        if (cm.height < 100)
                        {
                            cm.height += 0.5;
                            cm.width += 0.5;
                        }
                        if (cm.eggPeriod >= 100 && cm.height >= 100)
                        {
                            Chicks newChicks = new Chicks(cm);
                            chicksToAdd.Add(newChicks);
                            cm.eggPeriod = 0;
                        }
                        if (cm.toRotate)
                        {
                            cm.img = Properties.Resources.rchick;
                        }
                        else
                        {
                            cm.img = Properties.Resources.chick;
                        }
                        if (cm.posX < midLine || cm.posX  > endLine - 100)
                        {
                            cm.speedX = -cm.speedX;

                        }

                        if (cm.posY < 0 || cm.posY > bottom - 100)
                        {
                            if (cm.posY >= bottom)
                            {
                                cm.toRotate = true;
                            }
                            else
                            {
                                cm.toRotate = false;
                            }
                            cm.speedY = -cm.speedY;

                        }
                    }
                    else
                    {
                        cm.age++;
                    }
                    index += 1;

                }
                foreach (Chicks cm in chicksToAdd)
                {
                    chicks.Add(cm);
                }
                chicksToAdd.Clear();

                Food food = new Food();
                Foods.Add(food);
                this.Refresh();
                this.Invalidate();
            }
            catch 
            { 
                Console.WriteLine("Failed");
            }

           
        }

        #endregion

        #region DUCK TIMER
        private void duckTimer_Tick(object sender, EventArgs e)
        {
            try
            {
                int index = 0;

                foreach (Ducks cm in Ducks)
                {

                    if (!isEgg(cm.age))
                    {
                        cm.MoveAnimal();
                        cm.posX += cm.speedX;
                        cm.posY += cm.speedY;
                        cm.eggPeriod += 1;
                        if (cm.height < 100)
                        {
                            cm.height += 0.5;
                            cm.width += 0.5;
                        }
                        if (cm.eggPeriod >= 40 && cm.height >= 100  &&
                            isDuckInLand(cm.posX,cm.posY))
                        {
                            Ducks newDucks = new Ducks(cm);
                            DucksToAdd.Add(newDucks);
                            cm.eggPeriod = 0;
                        }
                        if (cm.toRotate)
                        {
                            cm.img = Properties.Resources.rduck;
                        }
                        else
                        {
                            cm.img = Properties.Resources.duck;
                        }
                        if (cm.posX < 100|| cm.posX > endLine - 100)
                        {
                            cm.speedX = -cm.speedX;
                        }

                        if (cm.posY < 0 || cm.posY > bottom - 100)
                        {
                            if (cm.posY >= bottom)
                            {
                                cm.toRotate = true;
                            }
                            else
                            {
                                cm.toRotate = false;
                            }
                            cm.speedY = -cm.speedY;

                        }
                    }
                    else
                    {
                        cm.age++;
                    }
                    index += 1;

                }
                foreach (Ducks cm in DucksToAdd)
                {
                    Ducks.Add(cm);
                }
                DucksToAdd.Clear();

                this.Refresh();
                this.Invalidate();
            }
            catch
            {
                Console.WriteLine("Failed");
            }
        }
        #endregion 
        #region FOOD TIMER
        private void foodTimer_Tick(object sender, EventArgs e)
        {
            CheckCollision();
        }
        #endregion
        #endregion

        #region USER DEFINE FUNCTION
        private bool isEgg(int age)
        {
            if (age < 100)
            {
                return true;
            }
            else { return false; }

        }
        private double findDistance(int x1, int y1, int x2, int y2)
        {
            return Math.Sqrt((x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2));
        }

        bool isDuckInLand(int x, int y)
        {
            if(x>Width/2)
                return true;
            return false;
        }

        private void CheckCollision()
        {
            if (Foods.Count > 0)
            {
                foreach (Food food in Foods)
                {
                    food.checkLifetime();
                    if (food.expired)
                    {
                        FoodsToRemove.Add(food);
                    }
                    bool collision = false;
                    foreach (Chicks c in chicks)
                    {
                        collision |=
                            DetectCollision(food.posX, food.posY, food.width, food.hight,
                            c.posX, c.posY, (int)c.width, (int)c.height);
                    }
                    foreach (Ducks c in Ducks)
                    {
                        collision |=
                            DetectCollision(food.posX, food.posY, food.width, food.hight,
                            c.posX, c.posY, (int)c.width, (int)c.height);
                    }
                    if (collision)
                    {
                        FoodsToRemove.Add(food); ;
                    }
                }
                foreach(Food f in FoodsToRemove)
                {
                    Foods.Remove(f);
                }
                FoodsToRemove.Clear();
            }
           
        }

        private bool DetectCollision(int object1X, int object1Y, int object1Width, int object1Height, int object2X, int object2Y, int object2Width, int object2Height)
        {
            if (object1X + object1Width <= object2X || object1X >= object2X + object2Width || object1Y + object1Height <= object2Y || object1Y >= object2Y + object2Height)
            {
                return false; 
            }
            else
            {
                return true; 
            }
        }
        #endregion
    }
}
```