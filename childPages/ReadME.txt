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
// Global Variables
Minim minim; // Object to access all functions
AudioPlayer[] playList; // Array for music files
AudioPlayer[] soundEffects; // Array for sound effects

int numberSoundEffects = 4;
int numberMusicSongs = 8;

int currentSong = 0; // Track index for playing song
int appWidth, appHeight;
int size;
PFont generalFont;
String quit = "QUIT";

boolean looping = false; // Protects .rewind in draw() from being inappropriately accessed
boolean dayMode = false; // App starts in Night Mode, set to day in setup()
boolean lightMode = false; // Dark mode starts App, null possible if USER Preferences made

color white = 255, yellow = #FFFF00, black = 0, purple = #FF00FF;
color backgroundColour, darkBackground = 0, whiteBackground = 255; // Gray Scale
color foregroundColour;

String pathDarkBackgroundImage, pathLightBackgroundImage;
PImage space, backgroundImage, albumCoverImage;
float albumCoverRIGHT, albumCoverCENTERED, albumCoverLEFT;

float backgroundX, backgroundY, backgroundWidth, backgroundHeight;
float albumCoverX, albumCoverY, albumCoverWidth, albumCoverHeight;
float playButtonX, playButtonY, playButtonWidth, playButtonHeight;
float quitButtonX, quitButtonY, quitButtonWidth, quitButtonHeight;
float progressBarX, progressBarY, progressBarWidth, progressBarHeight;
float timeDisplayX, timeDisplayY, timeDisplayWidth, timeDisplayHeight;

boolean isPlaying = false;
int elapsedTime = 0; // In seconds
int totalDuration = 240; // Total duration of the song in seconds (4 minutes)

String pathLightBackgroundImage = "path_to_light_background_image.jpg";
String pathDarkBackgroundImage = "path_to_dark_background_image.jpg";

void setup() {
  size(600, 400);
  appWidth = displayWidth;
  appHeight = displayHeight;
  minim = new Minim(this);

  playList = new AudioPlayer[numberMusicSongs];
  soundEffects = new AudioPlayer[numberSoundEffects];

  String pathSoundEffects = "../../../SoundFileSoundEffect/";
  String pathMusic = "../../../SoundFile/MusicDownload/";
  String quitButtonSound = "The_Simplest_sting";
  String groove = "groove";
  String extension = ".mp3";

  soundEffects[0] = minim.loadFile(pathSoundEffects + quitButtonSound + extension);
  playList[0] = minim.loadFile(pathMusic + groove + extension);

  generalFont = createFont("BodoniMT-italic", 16);

  String albumCoverImagePath = "../../../images/2.-Ed-Smith-1800x1200.jpg";
  albumCoverImage = loadImage(albumCoverImagePath);

  population();

  String backgroundImageName = "space-7978460_1280.jpg";
  pathLightBackgroundImage = "../../../../Images/backgroundimage/" + backgroundImageName;
  pathDarkBackgroundImage = "../../../../Images/backgroundimage/" + backgroundImageName;

  if (hour() >= 9 && hour() <= 17) dayMode = true;
  if (dayMode == true && lightMode == true) {
    backgroundColour = whiteBackground;
    foregroundColour = black;
    backgroundImage = loadImage(pathLightBackgroundImage);
  } else if (lightMode == false) {
    backgroundColour = black;
    foregroundColour = whiteBackground;
    backgroundImage = loadImage(pathDarkBackgroundImage);
  } else {
    backgroundColour = darkBackground;
    foregroundColour = yellow;
    backgroundImage = loadImage(pathDarkBackgroundImage);
  }
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
  fill(purple);
  rect(quitButtonX, quitButtonY, quitButtonWidth, quitButtonHeight);
  if (mouseX > quitButtonX && mouseX < quitButtonX + quitButtonWidth && mouseY > quitButtonY && mouseY < quitButtonY + quitButtonHeight) {
    fill(yellow);
    rect(quitButtonX + quitButtonWidth * 1 / 7, quitButtonY + quitButtonHeight * 1 / 7, quitButtonWidth * 5 / 7, quitButtonHeight * 5 / 7);
  } else {
    fill(purple);
  }

  // Quit button text
  fill(foregroundColour); // Resetting the Defaults
  textAlign(CENTER, CENTER);
  textSize(height * 1 / 23);
  text(quit, quitButtonX + quitButtonWidth * 1 / 7 + quitButtonWidth * 5 / 14, quitButtonY + quitButtonHeight * 1 / 7 + quitButtonHeight * 5 / 14);

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
  if (mouseX > playButtonX && mouseX < playButtonX + playButtonWidth && mouseY > playButtonY && mouseY < playButtonY + playButtonHeight) {
    isPlaying = !isPlaying;
  }

  if (mouseX > quitButtonX && mouseX < quitButtonX + quitButtonWidth && mouseY > quitButtonY && mouseY < quitButtonY + quitButtonHeight) {
    exit(); // Close the application
  }
}

void keyPressed() {
  if (key == 'Q' || key == 'q') {
    soundEffects[0].play();
  }
  if (key == CODED && keyCode == ESC) { // Hardcoded QUIT, no sound available
    soundEffects[0].play();
  }
  if (key == 'W' || key == 'w') { // Day Mode, White Light Containing Blue Colour
