Project 2: iCast

Group 7: Aditya Pawar, William Thomas, Nate Heath, Elliot Warner, Raihan Rafeek

Project Description:

iCast serves to be the User-Interface for making casts smart, fun, and more efficient. The casts that we are used to right now provide a conventional recovery process which involves putting on an immovable cast which is prone to external environmental factors such as humidity which can result in a soggy cast. In the case of your healing plan being changed— you would likely head into the doctor’s office to get the cast tightened, but iCast strives to solve all of these problems through a simple UI while keeping the nostalgia of having a cast!

**Design:**

Affordances:

1)       The cast fits on a wrist and offers support while healing

2)       The display invites the user to swipe/press

3)       The display allows the user to see real-time health data

4)       Since the user is restricted to using one hand, it relies on single taps and swipes.

Interviews:

In order to capture user needs, it was important that we interviewed people from different backgrounds to get their perspectives on them. Luckily for us, we were able to interview 3 people with distinct technical knowledge which could potentially corelate to our project.

Interview 1: Cameron Pocisk- 4th Year Computer Science Major

Overall summary of Interview:

We asked Cameron what his frustrations with traditional casts were and he responded by saying that it’s not possible to understand what’s happening under it— if it’s too tight, too humid, or if our skin is irritated. Secondly, in order to get his perspective of how a cast could be made smart— he said that he would like having sensors that monitor his health and also to get data on the recovery progress! Lastly, we asked for his input on the interface and how it can be slick to use— his response was that it shouldn’t be complicated or cluttered as we only have one hand to use to guide ourselves through the UI.

Takeaway: Cameron emphasized the need to get data from within the cast, which we can’t get an idea about thanks to the cast itself.

Interview 2: Caroline Risimini- 2nd year Interior Design Major

Overall summary of Interview:

For Caroline, we were excited to hear her perspectives on how we make a cast look trendy as she’s an Interior Design major. For our first question, we asked her how her experience with casts has been like— she responded by saying that they’re ugly and uncomfortable and don’t have a persona as they’re so generic. Secondly, we asked her if she had a smart cast, how would she want it to be— she responded by saying that if it could be customized per the user’s aesthetic and have some personality which is added by signing the casts by friends and family, that would be awesome! Finally, she recommended that our interface should be pretty calming as stressing the user out by having a lot of colors is not the right path to take for recovery!  
  
Takeaway: Caroline highlighted how important it is to make sure that each cast can stand out from each other and doesn’t end up being generic.

Interview 3: Allison Covery- 4th year Architectural Engineering Major

Overall summary of Interview: Our first question was what Allison currently dislikes about traditional casts— she responded by saying that they’re uncomfortable and there’s no way to change the amount of pressure the cast applies as it’s a literal cast mold. This highlighted the need for changeable cast pressure as per the recovery process. Secondly, we asked her what features she would want to see in a smart cast— she responded by highlighting the idea of Pressure Control which can send data to the doctors so it would be more efficient for both parties. When we asked her about input/interactions with the cast— she responded by saying that having buttons or sliders would be a great way to minimize the amount of precise touch that is required. For example, we wouldn’t want the user to be typing in the amount of pressure change required— it would be much more efficient to use arrows or slider controls for the same.

Takeaway: Allison highlighted the need for keeping the inputs simple and easy to use rather than making it complex while also giving the idea of a changeable cast pressure.

Assumptions:

1)       Assume that the cast can accurately track the humidity within the cast

2)       Assume that the UV light within the cast effectively eliminates bacteria

3)       Assume that adjusting the pressure within the cast changes the pressure on the bone

4)       Assume that the heartrate tracking is accurate to the user

5)       Assume that the signatures are written using the smart cast and saved on the device

6)       Assume that the recovery percentage is accurate, counting down the time until the bone is fully healed

Needs and requirements:

1)       The user needs to be able to monitor their heart rate, humidity and temperature

2)       The user needs to be able to view previous signatures from others

3)       The user needs to be able to adjust the pressure of the cast

4)       The user needs to be able to view their bone using an x-ray

5)       The user needs to be able to sanitize their cast using UV

  

**Sketches:**

  <img width="975" height="531" alt="image" src="https://github.com/user-attachments/assets/3dc9d6a2-ea09-4648-8694-3fa4a9ee7474" />
  <img width="975" height="731" alt="image" src="https://github.com/user-attachments/assets/a1e65f75-4c53-4594-a579-9b8377e172ef" />
  <img width="975" height="780" alt="image" src="https://github.com/user-attachments/assets/a8c9b87d-9c53-4a44-9212-b8f154e3da0d" />
  <img width="975" height="548" alt="image" src="https://github.com/user-attachments/assets/849f0823-50d8-4464-9a1d-affec4accdd6" />
  <img width="975" height="583" alt="image" src="https://github.com/user-attachments/assets/3b70c997-c414-460d-a5d1-cc8bf9966b00" />

Hybrid Sketch:
  <img width="671" height="608" alt="image" src="https://github.com/user-attachments/assets/c74f1cd8-969f-445a-a227-9f5a228a1b7c" />

Feedback:

The biggest concern we seemed to get with the initial sketches was that they looked too cluttered. Elliot asked his dad about the first two sketches, and that was one of the major things they commented on improving. As you can see from the later sketches and even the hybrid drawing, one of the main things that changed was the layout of the screen. The position on the arm stayed roughly the same, but the actual content got a large overhaul. As we iterated and worked with changing the content, we were able to get good responses from past people we interviewed was that the tabular design seemed to be intuitive and the most natural with how people were used to working with interfaces.

**Documentation:**

Our project is a smart cast, where a user can track and control different features to aid their healing. This is done with features such as heart-rate monitoring, UV cleaning, an X-Ray, and changing the pressure of the cast. This would be a helpful tool for those who may have an injury and are looking for other ways to support their healing process.

**Design Work:**

The interface consists of 4 main pages: Health, Signatures, UV/X-Ray, and Pressure. The health page is the main dashboard showcasing all important information. It shows the status of your recovery journey, your heart rate, the strength of the bone, your body temperature, and the humidity inside the cast. The humidity also functions as a critical safety feature as it tells you if the device is too wet. This feature can be seen by interacting with shower controls. You can also see the BPM change by interacting with the +, - and reset BPM buttons.

<img width="853" height="456" alt="image" src="https://github.com/user-attachments/assets/2280c644-0ae4-453e-a1d1-a7cabb74e117" />
<img width="858" height="458" alt="image" src="https://github.com/user-attachments/assets/7f006cbd-cab4-4e66-841a-e4fb46a54801" />

The signature page is a slideshow format page on the display which shuffles between different signatures sent to the cast by your loved ones. It capitalizes the entire screen space to show the signature and also shows the name of who sent the signature at the bottom.

<img width="817" height="438" alt="image" src="https://github.com/user-attachments/assets/d8a1d19f-9f32-4a71-8d32-02fc1af9ec71" />

The UV/X-ray slide houses two features of the cast: The UV cleaner and the X-ray. The UV cleaner screen houses a timer and two button controls. The start UV clean button starts the cleaning procedure within the cast, and a timer on the screen to signal that the process has started. Once the timer runs out, it shows that the cleaning procedure is complete.

<img width="781" height="416" alt="image" src="https://github.com/user-attachments/assets/95ddd546-5eda-4392-b247-31d1a9351e9c" />

When the start x-ray button is pressed, the sensor then runs a complete scan of the bone underneath the cast and shows the results live on the screen. The resulting x-ray helps the user keep track of their recovery journey.

<img width="786" height="419" alt="image" src="https://github.com/user-attachments/assets/8338060a-eea4-4224-8741-dd88a0583975" />

The pressure screen helps increase and decrease the tightness of the cast on the arm. The screen also shows a graph with patterns of how the pressure has been increased or decreased. The min and max pressure text helps keep track of how high or low the pressure has been set to. When the target pressure is adjusted using the arrows, you can see the current pressure increase with time.

<img width="808" height="433" alt="image" src="https://github.com/user-attachments/assets/6fad8a33-c98c-49eb-9f0a-02ebe7eb9be5" />

If the screen is left unattended for over 5 seconds, the display goes into a screensaver mode. The screensaver shows the date and time in the dead center. The top of the screen shows a motivational quote to keep the user’s spirits up. The bottom of the screen shuffles through the signatures the user’s loved ones send.

<img width="858" height="456" alt="image" src="https://github.com/user-attachments/assets/e593d1f0-f40e-4eed-bc6d-adb7f5837d61" />

**Implementation:**

For this project we used the Svelte framework with JS, HTML, CSS. Most of us used the VS Code application to build our website. We used Vite to create our project directory structure and just built off of the generated skeleton code.

We used a Svelte component for each page, switching between them as seen below:  

<img width="703" height="282" alt="image" src="https://github.com/user-attachments/assets/e4659050-ecfb-4c5b-9bc3-8bdb57d8fc2a" />

We didn’t use any external libraries, but we used imported Svelte methods. We used onMount and onDestroy in most components, and we also used the fade transition from Svelte. Some examples of such are below:

<img width="706" height="204" alt="image" src="https://github.com/user-attachments/assets/ed8aab94-2097-44ad-be1c-0a06e71aa137" />
<img width="975" height="332" alt="image" src="https://github.com/user-attachments/assets/ba9ef584-9773-400e-9706-5b1bf2a1d45f" />
<img width="975" height="28" alt="image" src="https://github.com/user-attachments/assets/497c6bb9-ef9a-4940-a0ea-193786e6de5a" />

The screenshot directly above outlines a use case of the fade transition, seen at the end of this line.

As far as code structure goes, we followed a pretty standard Svelte pattern of placing the script at the top of the file, followed by the HTML, then the styling.

Finally, we used GitHub for source control so we could work in independent branches without stepping on each other’s toes. We used Vercel to host the website.

**Future Work:**

In the future, there are a few features that we would like to implement. Giving users a way to add signatures would be important for us. Currently, we have the signatures hardcoded into the application, but people obviously would like to have friends/family members sign their cast in real-time. We would also like to implement more customization features; in real life, you can choose the color of your cast, so giving the users the option to change the color of their cast would allow for that same level of personalization.

It would also be nice if we could have a feature that tracked patterns and history of movement, activity, trauma or impact to the cast, etc., to help with the medical diagnosis aspect of this treatment and recovery tracking.

**Links:**

Demo: [https://www.youtube.com/watch?v=dz-PW5aLvoY](https://www.youtube.com/watch?v=dz-PW5aLvoY)

Code: [https://github.com/Rai1975/iCast---UI-Project-2](https://github.com/Rai1975/iCast---UI-Project-2)

Website: [https://icast-ui-project2.vercel.app/](https://icast-ui-project2.vercel.app/)

