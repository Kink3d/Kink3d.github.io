Writing Unit Tests for Image Effects at Unity
==================

A lot of game devs do not Unit Test…. and even more of them do not make graphics tests to make sure updated sprites, textures, and more don;t ruin your game and the look you are working for.

I wish I could say this is going to be a hard tutorial but…. its so easy…. The steps are as follows: 

1. Create Unit Test Folder

2. Create Unit Test - Takes Screenshot, saves to folder location

3. Create Scene, Drag Shader and Script onto camera, Compare images. 

—————————-

Some of the things we will need before these tests become USEFUL are control images. 

1. Images that you will be comparing future versions of your game to. 

2. Keep doing these tests before every single check in that changes how your game looks in ANYWAY! It is important to do this before you check in your code to version control so if you broke something, someone else doesn’t get screwed over. 

—————————–

Alright…. Lets get to it! 

—————————– 

Preface
These Unit tests can be done at Runtime on Standalone builds but in this we will be doing Unit Tests in the Unit Test runner directly in the Editor. I’ll show you how to do these at Runtime later but you’ll kind of roll your eyes when you realize how easy this is. 

Setup
1. Open Unity

2. Navigate to the project you are working on. 

3. Open a Scene. Pick any scene. The test we are going to write will work with Editor Test Runner and at Runtime with a timer.

4. Window-> Editor Test Runner. Dock it somewhere in Unity. 

5. Import this the GFX comparing into your Unity Project. Its 1 Shader, 3 scripts. all inside a Unit Tests Folder. Download Here

6. Your project should now look something like this:

![after import](http://68.media.tumblr.com/cca329746cf20a89b2a46a19284e67d4/tumblr_inline_oe89f7ludh1qm6iz9_500.png)


7. Create a new folder called “Tests” under UnitTests->Editor->Tests. All Unit Tests must be under an Editor Folder if you run them in the Editor due to using the UnityEditor namespace. 

8. Right-Click the Tests folder and select Create->Editor Test and name the file “SS_Test”

9. The test will look something like this: 

~~~ csharp
using UnityEngine;
using UnityEditor;
using NUnit.Framework;

public class SS_Tut {

[Test]
public void EditorTest() {
//Arrange
var gameObject = new GameObject();

//Act
//Try to rename the GameObject
var newGameObjectName = “My game object”;
gameObject.name = newGameObjectName;

//Assert
//The object has a new name
Assert.AreEqual(newGameObjectName, gameObject.name);
}
}
~~~


we want to delete all of that stuff because we need none of it… so it should look like this: 

~~~ csharp

using UnityEngine;
using UnityEditor;
using NUnit.Framework;

public class SS_Tut {

[Test]
public void EditorTest() {

}
}

Rename the default method to be “SceneSS”

using UnityEngine;
using UnityEditor;
using NUnit.Framework;

public class SS_Tut {

[Test]
public void SceneSS() {

}
}

~~~

Notice some new things here that you might of have not scene before. 

[Test] displays above methods. This needs to be there so the Editor Test Runner knows what tests to run.

using NUnit.Framework is the framework Unity uses to Unit Test. 



Creating A Test
Now the following is what we want to do to make a GFX Unit Test. 

1. Take a Screenshot of our scene

2. Make some change to how the game looks, weather it will be a texture or shader. 

3. Compare them. 



I am going to write the script for you and document what it does. Its the most simple script you have ever seen….. 

~~~ csharp

using UnityEngine;
using UnityEditor;
using NUnit.Framework;

public class SS_Tut {

private string m_fileName = “FirstTest”;

[Test]
public void SceneSS() {

//public static void CaptureScreenshot(string filename, int superSize = 0);
//save the file into the Assets folder as a png
Application.CaptureScreenshot(Application.dataPath + “/” + m_fileName + “.png”, 2);

}

}

~~~

Right? I know…. hold on… wait for it…. Now open up the editor test runner and unfold your tests. It should look something like this:


Now make sure you have the scene you want open and click Run All Tests. Everything should turn green! Like the following: 




But the screenshot hasn’t shown up in your assets folder. You have to Right-Click in your Project View and select “Refresh”. Now the Screenshot is displayed! WOW! So fucking easy! lol

The screenshot we just took will be the control image. This is what we will compare all future versions of our game to! So lets make a quick change by adding a cube: 


Now we are getting crazy! 

So lets change the name of our screenshot in our script to “version1”. Your script has a string variable that allows you to change the name. 

Now Run the Editor Test again to get a new screenshot. Dont forget to Refresh your project view! 

Comparing
Now we have the following: 

- Control image

- New Updated version with an awesome cube

Lets Setup our comparison scene. 

1. Create a new scene, save it under Unit Tests or where ever you want. You can just leave the default stuff in the scene. 

2. Select the Main Camera. 

3. Add a script called “ImageComparision” to your Main Camera

4. Notice the screen turns black and there are two slots for two images. Add your control Image to A and your version1 image to B. Should look something like this: 


Yay! Its showing you the difference in the two images. Here is a more complex example: 

Control Image: 






Updated Image: 




Comparison: 




Can you spot the differences before you even saw the compared scene? See how the littlest of things can really mess up your game? 



The best practice is to do this before checking in your code! 
FAQ: 

Q: How do I do this at Runtime so I can wait a set amount of seconds? 

A. Application.CaptureScreenshot() is really cool and works at Runtime. So if you make a script that times your screenshot using a Corroutine or Invoke() you can then do this anywhere in your game. 



THIS IS THE MOST BASIC GRAPHICS TEST. I MEAN … BASIC… there are better ways and more complex ways of doing this. But for those that aren’t doing this… I bet its most of you… this is a great way to get you thinking. 



Docs for Editor Test runner are here for more information: https://docs.unity3d.com/Manual/testing-editortestsrunner.html
