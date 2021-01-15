playSound("The-Medieval-Banquet-by-Silverma-(3).mp3", true);
var time = 60;
var xKnight = 100;
var yKnight = 100;
var KnightAlive = true;
var villagerCollision = false;
var skeletonCollision = false;
var SwordStatus = "lowered";
var Name = "";
var skeletonSlain = 0;
var skeletonEscaped = 0;
var villagerSlain = 0;
var villagerRescued = 0;
var totalScore = 0;
var eventList = [];
onEvent("aboutBack", "click", function( ) {
  setScreen("homeScreen");
});
onEvent("aboutButton", "click", function( ) {
  setScreen("aboutScreen");
});
onEvent("nameInput", "input", function() {
	Name = getText("nameInput");
	console.log(Name);
	setText("currentStats", "Player: " + Name);
});
onEvent("Start", "click", function() {
  time = 60;
	setScreen("gameScreen");
	setText("scoreGameDisplay", "Score: "+totalScore);
	mobSpawn("Villager", 25, 250);
	mobSpawn("Skeleton", 0, 500);
	Countdown();
});
onEvent("instructions", "click", function() {
  setScreen("instructionScreen");
});
onEvent("instructionBack", "click", function() {
  setScreen("homeScreen");
});
onEvent("restart", "click", function() {
	xKnight = 100;
	yKnight = 100;
	KnightAlive = true;
	SwordStatus = "lowered";
	Name = getText("nameInput");
	skeletonSlain = 0;
	skeletonEscaped = 0;
	villagerSlain = 0;
	villagerRescued = 0;
	totalScore = 0;
	setScreen("homeScreen");
	setText("scoreGameDisplay", "Score: "+totalScore);
});
onEvent("gameScreen", "mousemove", function(event) {
	if (KnightAlive === true) {
	  appendItem(eventList, event);
	  xKnight = event.x - 50;
	  yKnight = event.y - 50;
	  setPosition("Knight", xKnight, yKnight);
	  setPosition("Crossbones", xKnight, yKnight);
	  setPosition("Sword", xKnight + 55, yKnight - 55);
	  console.log(event);
	}
});
onEvent("gameScreen", "keydown", function(event) {
	if (event.key == " ") {
		Stance();
	}
});
onEvent("gameScreen", "click", function() {
  Stance();
});

function Countdown() {
	timedLoop(1000, function() {
		setText("timer", time + " seconds left");
		time = time - 1;
		if (time == 0) {
			stopTimedLoop();
			endGame();
			playSound("Applause-sound-effect.mp3", false);
		}
		if (KnightAlive == false) {
		  stopTimedLoop();
		  hideElement("Knight");
		  hideElement("Sword");
		  showElement("Crossbones");
		  showElement("YouDied");
		  setTimeout(function() {
		    hideElement("Crossbones");
		    hideElement("YouDied");
		    showElement("Knight");
		    endGame();
		  }, 2000);
		}
	});
}

function Stance() {
	playSound("Sword-Whoosh-Sound-Effect-Second-Variation.mp3", false);
	if (SwordStatus == "lowered") {
		showElement("Sword");
		SwordStatus = "raised";
	} else {
		hideElement("Sword");
		SwordStatus = "lowered";
	}
}

function mobSpawn(mob, style, interval) {
	var yMobSpawn = -100;
	if (mob == "Villager") {
		yMobSpawn = -50;
	}
	setPosition(mob, randomNumber(0, 220), yMobSpawn);
	timedLoop(interval, function() {
		collisionListen("Sword", "Skeleton");
		collisionListen("Sword", "Villager");
		collisionListen("Knight", "Skeleton");
		collisionListen("Knight", "Villager");
		setPosition(mob, getXPosition(mob) + randomNumber(-style, style), getYPosition(mob) + 10);
		if (getXPosition(mob) < -50 || getXPosition(mob) > 320 || getYPosition(mob) > 450) {
			if (mob == "Skeleton") {
			  skeletonEscaped++;
			  console.log("A skeleton escaped!");
			  	totalScore = totalScore - 1;
			  	setText("scoreGameDisplay", "Score: "+totalScore);
			}
			setPosition(mob, randomNumber(0, 220), yMobSpawn);
		}
		if (skeletonCollision == true) {
		  setPosition("Skeleton", randomNumber(0, 220), yMobSpawn);
		  skeletonCollision = false;
		}
		if (villagerCollision == true) {
		  setPosition("Villager", randomNumber(0, 220), yMobSpawn);
		  villagerCollision = false;
		}
	});
}

function collisionListen(object, mob) {
	var objectX = getXPosition(object);
	var objectY = getYPosition(object);
	var objectWidth = getProperty(object, "width");
	var objectHeight = getProperty(object, "height");
	var mobX = getXPosition(mob);
	var mobY = getYPosition(mob);
	var mobWidth = getProperty(mob, "width");
	var mobHeight = getProperty(mob, "height");
	if (objectX + objectWidth >= mobX && objectX <= mobX + mobWidth) {
		if (objectY + objectHeight >= mobY && objectY <= mobY + mobHeight) {
			if (KnightAlive == true) {
				if (object == "Sword") {
					if (SwordStatus == "raised") {
						if (mob == "Skeleton") {
							skeletonCollision = true;
							playSound("bone-crack-sound-effect_rDCz8kEq.mp3", false);
							skeletonSlain++;
							console.log("Skeleton slain!");
							totalScore = totalScore + 1;
							setText("scoreGameDisplay", "Score: "+totalScore);
						} else {
							villagerCollision = true;
							playSound("Wilhelm-Scream-sound-effect.mp3", false);
							villagerSlain++;
							console.log("Villager slain!");
							totalScore = totalScore - 2;
							setText("scoreGameDisplay", "Score: "+totalScore);
						}
					}
				} else {
					if (mob == "Skeleton") {
						skeletonCollision = true;
						playSound("Roblox-Death-Sound---OOF-Sound-Effect.mp3", false);
						KnightAlive = false;
						console.log("Knight slain!");
					} else {
						villagerCollision = true;
						playSound("yahoo-sound-effect_6oNI5J21.mp3", false);
						villagerRescued++;
						console.log("Villager rescued!");
						totalScore = totalScore + 1;
						setText("scoreGameDisplay", "Score: "+totalScore);
					}
				}
			}
		}
	}
}
function endGame() {
  setScreen("finishScreen");
  setText("finishTitle", "Total Score: "+totalScore);
  setText("ssStatBoard", "Skeletons slain: "+skeletonSlain);
  setText("seStatBoard", "Skeletons escaped: "+skeletonEscaped);
  setText("vsStatBoard", "Villagers slain: "+villagerSlain);
  setText("vrStatBoard", "Villagers rescued: "+villagerRescued);
  if (totalScore >= 0) {
    if (KnightAlive == true) {
      setText("HearYeBoard", "The noble knight "+Name+" hath saved the day and shall be revered forever henceforth!");
    } else {
      setText("HearYeBoard", "The noble knight "+Name+" wast slain in battle and shall be honored for their deeds!");
    }
    
  }else {
    if (KnightAlive == true) {
      setText("HearYeBoard", "The vile knight "+Name+" shall no longer be welcome in these lands.");
    } else {
      setText("HearYeBoard", "The vile knight "+Name+" wast slain in battle! Good riddance!");
    }
  }
}
