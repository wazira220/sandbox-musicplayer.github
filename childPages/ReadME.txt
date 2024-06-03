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
