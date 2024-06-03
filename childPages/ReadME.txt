/* Documentation
 Library: use Sketch / Import Library / Add Library / Minim
 Suporting Website: https://code.compartmental.net/minim/
 - https://code.compartmental.net/minim/audioplayer_method_loop.html
 - loop(0) seems best for sound effects
 */
//Library
import ddf.minim.*;
import ddf.minim.analysis.*;
import ddf.minim.effects.*;
import ddf.minim.signals.*;
import ddf.minim.spi.*;
import ddf.minim.ugens.*;
//
//Global Variables
Minim minim; //creates object to access all functions
AudioPlayer playList1; //creates "Play List" variable holding extensions WAV, AIFF, AU, SND, and MP3
AudioPlayer soundEffects1; //"Play List" for Sound Effects
Minim minim; //creates object to access all functions
int numberSoundEffects = 4; //DEV Verify, OS able to count (CS20 Solution)
int numberMusicSongs = 8; //DEV Verify, OS able to count (CS20 Solution)
AudioPlayer[] playList = new AudioPlayer[ numberMusicSongs ]; //creates "Play List" variable holding extensions WAV, AIFF, AU, SND, and MP3
AudioPlayer[] soundEffects = new AudioPlayer[ numberSoundEffects ]; //"Play List" for Sound Effects
int currentSong = 0; //JAVA starts counting at 0, not for all languages
int appWidth, appHeight, brightness=255;
float backgroundImageX, backgroundImageY, backgroundImageWidth, backgroundImageHeight;
PImage backgroundImage;
Boolean lightMode=true, starWars=false, nightMode=false;
//
int appWidth, appHeight;
//
int size;
PFont generalFont;
String quit="QUIT";
//
Boolean looping=false;
//Protects .rewind in draw() from being inappropriately accessed between .play(), .loop(1), & .loop()
color white=255, yellow=#FFFF00, black=0, purple=#FF00FF; //Hexidecimal, see Tools / Colour Selector
Boolean dayMode=false; //App starts in Night Mode, set to day in setup()
Boolean lightMode=false; //Dark mode starts App, null possible if USER Preferences made
//
color white=255, yellow=#FFFF00, black=0, purple=#FF00FF; //Hexidecimal, see Tools / Colour Selector
Boolean dayMode=false; //App starts in Night Mode, set to day in setup()
Boolean lightMode=false; //Dark mode starts App, null possible if USER Preferences made
//
color backgroundColour, darkBackground=0, whiteBackground=255; //Gray Scale, note much smaller than COLOR
color foregroundColour;
//
String pathDarkBackgroundImage, pathLightBackgroundImage;
PImage space,
PImage backgroundImageName;
PImage albumCoverImage;
float albumCoverRIGHT, albumCoverCENTERED, albumCoverLEFT;
//
void setup() {
  //Display
  size(600, 400); //width, height //400, 500
  //fullScreen(); //displayWidth, displayHeight
  appWidth = displayWidth; //width
  appHeight = displayHeight; //height
  //Landscape is HARDCODED
  String displayInstructions = ( appWidth >= appHeight ) ? "Good To Go" : "Bru, turn your phun";
  println(displayInstructions);
  //
minim = new Minim(this); //load from data directory, loadFile should also load from project folder, like loadImage
  String pathwaySoundEffects = "../../../SoundFileSoundEffect/";
  String pathwayMusic = "../../../SoundFile/MusicDownload/"; //Relative Path
  String quitButtonSound = "The_Simplest_sting";
  String groove = "groove";
  String extension = ".mp3";
    //println ( pathwaySoundEffects+quitButtonSound+extension );
  //println ( "Relative Pathway:", pathwayMusic+groove+extension );
  String pathQuitButtonSound = sketchPath( pathwaySoundEffects + quitButtonSound + extension ); //Absolute Path
  String pathGrooveSong = sketchPath( pathwayMusic + groove + extension ); //Absolute Path
  //println ( "Absolute Pathway:", pathGrooveSong ); //pathQuitButtonSound
  soundEffects[0] = minim.loadFile( pathQuitButtonSound );
  playList[0] =  minim.loadFile( pathGrooveSong ); // "" is compiler error
  //
  soundEffects1 = minim.loadFile( path );
  //playList1 = minim.loadFile( path );
  //
  //Fonts from OS (Operating System)
  //String[] fontList = PFont.list(); //To list all fonts available on OS
  //printArray(fontList); //For listing all possible fonts to choose from, then createFont
  size = ( appWidth > appHeight ) ? appHeight : appWidth ; // Font size starts with smaller dimension
  generalFont = createFont("BodoniMT-italic", size);
  //bottomFont = createFont("", size); //Note: more than one font allowed
  // Tools / Create Font / Find Font / Use size field / Do not press "OK", known bug
  //
  divs();
  //
  //Variable Population
  //Images
  String EdSmith = "2.-Ed-Smith-1800x1200";
String extensionJPG = ".jpg";
String pathway = "../../../images/";
String albumCoverImagePath = pathway + EdSmith + extensionJPG;
albumCoverImage = loadImage( albumCoverImagePath );
  //
  String backgroundImageName = "space-7978460_1280";
String extension = ".jpg";
String pathway = "../../../../Images/backgroundimage"
//Boolean darkMode=false; //See keyPressed for NOTE
//
  //Image Aspect Ratio Calculations
  //NOTE: IF-Else & WHILE to Adjust Aspect Ratio Dimensions
  //Forms a Procedure for Aspect Ratios of all Images ( copy and paste in setup() )
  float smallerAlbumCoverDimension = ( albumCoverWidth < albumCoverHeight ) ? albumCoverWidth : albumCoverHeight ;
  float albumCoverImageWidthPixel = 800.0; //Origonally INTs, ratio will be decimal
  float albumCoverImageHeightPixel = 600.0; //CAUTION: must avoid truncation to ZERO Value
  float albumCoverAspectRatio = albumCoverImageWidthPixel/albumCoverImageHeightPixel;
  float largerAlbumCoverDimension = smallerAlbumCoverDimension*albumCoverAspectRatio; //Apsect Ratio
  if ( albumCoverWidth < largerAlbumCoverDimension ) { //Image will not fit into DIV rect()
    while ( albumCoverWidth < largerAlbumCoverDimension ) {
      largerAlbumCoverDimension -= 1;
      smallerAlbumCoverDimension -= 1;
      //NOTE: ratios like percent are not linear decreases in both directions
    }
  }
  //
  /*Image can be centered, left justified, or right justified on the larger dimension
   LEFT: X-value of image same as rect()
   CENTERED: X-value of image = albumCoverX + (albumCoverWidth-albumCoverWidthAdjusted)/2;
   RIGHT: X-value of image = albumCoverX+albumCoverWidth-albumCoverWidthAdjusted;
   */
  albumCoverRIGHT = albumCoverX;
  albumCoverCENTERED = albumCoverX + (albumCoverWidth)/2;
  albumCoverLEFT =albumCoverX+albumCoverWidth;
  //
  //Time Calculations
  //if ( hour()>=9 && hour()<=17 ) backgroundColour = whiteBackground;
  //if ( hour()<9 && hour()>17 ) backgroundColour = darkBackground;
  if ( hour()>=9 && hour()<=17 ) dayMode=true; //Day & Night Mode Clock Choice
  println();
  if ( dayMode==true && lightMode==true ) { //Light & Dark Modes
    backgroundColour = whiteBackground;
    foregroundColour = black;
    backgroundImage = loadImage( pathLightBackgroundImage ); //Changing this Variable with 3 different images
  } else if ( lightMode==false ) {
    backgroundColour = black;
    foregroundColour = whiteBackground;
    backgroundImage = loadImage( pathDarkBackgroundImage );
  } else {
    backgroundColour = darkBackground;
    foregroundColour = yellow; //Note: if(hour()<9&&hour()>17)
    backgroundImage = loadImage( pathDarkBackgroundImage );
  }
  //
  //soundEffects1.loop();
} //End setup
//
void draw() {
  //Display
  // background(backgroundColour); //Hardcoded Backgorund Colour Out, use IF to change
  if ( dayMode=true && lightMode == true ) { //Boolean keyBind, Logical Shortcut
    //CAUTION: See setup
    backgroundImage = loadImage( pathLightBackgroundImage );
  } else if ( lightMode == false ) {
    backgroundImage = loadImage( pathDarkBackgroundImage );
  } else {
    tint(255, 255, 255, 0); //no blue;
  }
  image( backgroundImage, backgroundX, backgroundY, backgroundWidth, backgroundHeight);
  //fill(foregroundColour);
  //
  //Quit Button
  //fill(purple);
  //if ( mouseX>quitButtonX && mouseX<quitButtonX+quitButtonWidth && mouseY>quitButtonY && mouseY<quitButtonY+quitButtonHeight ) fill(yellow);
  fill(purple);
  rect(quitButtonX, quitButtonY, quitButtonWidth, quitButtonHeight);
  if ( mouseX>quitButtonX && mouseX<quitButtonX+quitButtonWidth && mouseY>quitButtonY && mouseY<quitButtonY+quitButtonHeight ) {
    fill(yellow);
    rect( quitButtonX+quitButtonWidth*1/7, quitButtonY+quitButtonHeight*1/7, quitButtonWidth*5/7, quitButtonHeight*5/7);
  } else {
    fill(purple);
  }
  fill(foregroundColour); //Resetting the Defaults
  //Quit, Text
  fill(foregroundColour); //Ink
  textAlign( CENTER, CENTER ); //Align X&Y, see Processing.org / Reference
  //Values: [ LEFT | CENTER | RIGHT ] & [ TOP | CENTER | BOTTOM | BASELINE ]
  size = appHeight*1/23; // Var based on ratio of display
  textFont(generalFont, size);
  text(quit, quitButtonX+quitButtonWidth*1/7, quitButtonY+quitButtonHeight*1/7, quitButtonWidth*5/7, quitButtonHeight*5/7); //Inside rect() above
  fill(foregroundColour); //Resetting the Defaults
  //
  //Albumn Cover Image
  image( albumCoverImage, albumCoverCENTERED, albumCoverY );
  //
  //println(mouseX, mouseY);
  //
} //End draw
//
void keyPressed() { //Listener
  if (key=='Q' || key=='q')
  {
    soundeffect_1();
  }
  if (key==CODED && keyCode==ESC) //Hardcoded QUIT, no sound available
  {
    soundeffect_1();
  }
  //CAUTION, must return to "Request White, Light Mode"
  if ( key=='W' || key=='w' ) { //Day Mode, White Light Containing Blue Colour
    if (  lightMode == false ) {
      lightMode = true;  //Light Mode ON
    } else {
      lightMode = false; //Dark Mode ON, no darkMode Boolean required
    }
  } //End Day Mode
  //
  //soundEffects1.loop(0);
} //End keyPressed
//
void mousePressed() { //Listener
  if ( mouseX>quitButtonX && mouseX<quitButtonX+quitButtonWidth && mouseY>quitButtonY && mouseY<quitButtonY+quitButtonHeight )
  {
    soundeffect_1();
  }
} //End mousePressed
//
void keyPressed() { //Key Board Short Cuts for Mouse Pressing Prototyping
  if ( key=='W' || key=='w' ) { //Day Mode, White Light Containing Blue Colour
    if (  lightMode == false ) {
      lightMode = true;  //Light Mode ON
    } else {
      lightMode = false; //Dark Mode ON, no darkMode Boolean required
    }
  } //End Day Mode
  //if () {} //End Night Mode
} //End keyPressed
//
//End MAIN Program
float backgroundX, backgroundY, backgroundWidth, backgroundHeight;
float albumCoverX, albumCoverY, albumCoverWidth, albumCoverHeight;
float playButtonX, playButtonY, playButtonWidth, playButtonHeight;
float quitButtonX, quitButtonY, quitButtonWidth, quitButtonHeight;
float progressBarX, progressBarY, progressBarWidth, progressBarHeight;
float timeDisplayX, timeDisplayY, timeDisplayWidth, timeDisplayHeight;

boolean isPlaying = false;
int elapsedTime = 0; // In seconds
int totalDuration = 240; // Total duration of the song in seconds (4 minutes)

PImage backgroundImage;
PImage albumCoverImage;

boolean dayMode = true;
boolean lightMode = true;

String pathLightBackgroundImage = "path_to_light_background_image.jpg";
String pathDarkBackgroundImage = "path_to_dark_background_image.jpg";

void setup() {
  size(400, 800);
  population();
  albumCoverImage = loadImage("album_cover.jpg"); // Make sure to have an album_cover.jpg in your data folder
  backgroundImage = loadImage(pathLightBackgroundImage); // Initial background image
}

void population() {
  backgroundX = 0;
  backgroundY = 0;
  backgroundWidth = width;
  backgroundHeight = height;

  albumCoverX = width * 18 / 100;
  albumCoverY = height * 15 / 100;
  albumCoverWidth = width * 64 / 100;
  albumCoverHeight = height * 6 / 16;

  playButtonX = width * 3.5 / 8;
  playButtonY = height * 8 / 9;
  playButtonWidth = width * 1 / 8;
  playButtonHeight = height * 1 / 12;

  quitButtonX = width * 90 / 100;
  quitButtonY = height * 91 / 100;
  quitButtonWidth = width * 9 / 100;
  quitButtonHeight = height * 8 / 100;

  progressBarX = width * 1 / 10;
  progressBarY = height * 7.5 / 10;
  progressBarWidth = width * 8 / 10;
  progressBarHeight = height * 1 / 30;

  timeDisplayX = width * 1 / 10;
  timeDisplayY = height * 7 / 10;
  timeDisplayWidth = width * 8 / 10;
  timeDisplayHeight = height * 1 / 20;
}

void draw() {
  // Display background image based on mode
  if (dayMode == true && lightMode == true) {
    backgroundImage = loadImage(pathLightBackgroundImage);
  } else if (lightMode == false) {
    backgroundImage = loadImage(pathDarkBackgroundImage);
  } else {
    tint(255, 255, 255, 0); // no image
  }
  image(backgroundImage, backgroundX, backgroundY, backgroundWidth, backgroundHeight);

  // Quit button
  fill(150, 0, 150); // Purple color
  rect(quitButtonX, quitButtonY, quitButtonWidth, quitButtonHeight);
  if (mouseX > quitButtonX && mouseX < quitButtonX + quitButtonWidth && mouseY > quitButtonY && mouseY < quitButtonY + quitButtonHeight) {
    fill(255, 255, 0); // Yellow color
    rect(quitButtonX + quitButtonWidth * 1 / 7, quitButtonY + quitButtonHeight * 1 / 7, quitButtonWidth * 5 / 7, quitButtonHeight * 5 / 7);
  } else {
    fill(150, 0, 150); // Purple color
  }

  // Quit button text
  fill(255); // White color
  textAlign(CENTER, CENTER);
  textSize(height * 1 / 23);
  text("QUIT", quitButtonX + quitButtonWidth * 1 / 7 + quitButtonWidth * 5 / 14, quitButtonY + quitButtonHeight * 1 / 7 + quitButtonHeight * 5 / 14);

  // Album cover image
  image(albumCoverImage, albumCoverX, albumCoverY, albumCoverWidth, albumCoverHeight);

  // Play/Pause button
  fill(150);
  rect(playButtonX, playButtonY, playButtonWidth, playButtonHeight);

  // Play/Pause symbol
  fill(255);
  if (isPlaying) {
    rect(playButtonX + playButtonWidth * 0.3, playButtonY + playButtonHeight * 0.2, playButtonWidth * 0.1, playButtonHeight * 0.6);
    rect(playButtonX + playButtonWidth * 0.6, playButtonY + playButtonHeight * 0.2, playButtonWidth * 0.1, playButtonHeight * 0.6);
  } else {
    triangle(playButtonX + playButtonWidth * 0.3, playButtonY + playButtonHeight * 0.2,
             playButtonX + playButtonWidth * 0.3, playButtonY + playButtonHeight * 0.8,
             playButtonX + playButtonWidth * 0.7, playButtonY + playButtonHeight * 0.5);
  }

  // Progress bar background
  fill(120);
  rect(progressBarX, progressBarY, progressBarWidth, progressBarHeight);

  // Progress bar foreground
  fill(0, 255, 0);
  rect(progressBarX, progressBarY, progressBarWidth * (float) elapsedTime / totalDuration, progressBarHeight);

  // Time display
  fill(0);
  textAlign(CENTER, CENTER);
  textSize(14);
  text(formatTime(elapsedTime) + " / " + formatTime(totalDuration), timeDisplayX + timeDisplayWidth / 2, timeDisplayY + timeDisplayHeight / 2);

  // Update elapsed time if playing
  if (isPlaying && frameCount % 60 == 0 && elapsedTime < totalDuration) {
    elapsedTime++;
  }
}

void mousePressed() {
  if (mouseX > playButtonX && mouseX < playButtonX + playButtonWidth &&
      mouseY > playButtonY && mouseY < playButtonY + playButtonHeight) {
    isPlaying = !isPlaying;
  }

  if (mouseX > quitButtonX && mouseX < quitButtonX + quitButtonWidth &&
      mouseY > quitButtonY && mouseY < quitButtonY + quitButtonHeight) {
    exit(); // Close the application
  }
}

String formatTime(int seconds) {
  int minutes = seconds / 60;
  int secs = seconds % 60;
  return nf(minutes, 2) + ":" + nf(secs, 2);
}
