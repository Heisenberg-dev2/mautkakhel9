<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Truth or Dare Wheel</title>
  <style>
    body {
      background: linear-gradient(to bottom right, #1f1c2c, #928dab);
      font-family: Arial, sans-serif;
      color: white;
      text-align: center;
      padding: 50px;
    }
    h1 {
      font-size: 3em;
    }
    #wheel {
      width: 300px;
      height: 300px;
      margin: 30px auto;
      border: 10px solid white;
      border-radius: 50%;
      position: relative;
      background: conic-gradient(#ff7675 0 33.3%, #74b9ff 33.3% 66.6%, #ffeaa7 66.6% 100%);
      transition: transform 3s ease-out;
    }
    #wheel:after {
      content: "";
      width: 20px;
      height: 20px;
      background: black;
      position: absolute;
      top: -30px;
      left: 140px;
      clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
    }
    button {
      padding: 15px 30px;
      font-size: 18px;
      background-color: #00cec9;
      border: none;
      border-radius: 10px;
      color: white;
      cursor: pointer;
      margin-top: 20px;
    }
    #result {
      font-size: 1.5em;
      margin-top: 30px;
      padding: 20px;
      border-radius: 10px;
      background-color: rgba(255,255,255,0.1);
      min-height: 100px;
    }
    .emoji {
      font-size: 60px;
      margin-top: 20px;
    }
  </style>
</head>
<body>

  <h1>🎯 Truth or Dare Wheel 🎯</h1>

  <div id="wheel"></div>
  <button onclick="spinWheel()">SPIN</button>
  <div id="result"></div>
  <div id="face" class="emoji"></div>

  <script>
    const truths = [
      "What's your most embarrassing moment?",
      "Have you ever lied to a friend?",
      "What's a secret you've never told anyone?",
      "Who was your first crush?",
      "Have you ever cheated in a game?",
      "Do you talk in your sleep?",
      "What’s your guilty pleasure?",
      "What’s your worst habit?",
      "Have you ever ghosted someone?",
      "What’s your weirdest dream?",
      "Have you ever had a crush on a teacher?",
      "What’s your biggest fear?",
      "What’s the last thing you Googled?",
      "Have you ever broken something and blamed someone else?",
      "Do you sing in the shower?",
      "What’s your biggest insecurity?",
      "Have you ever stolen anything?",
      "What’s your most awkward moment?",
      "If you had to delete one app forever, which would it be?",
      "Have you ever peed in a pool?",
      "What’s your worst fashion choice?",
      "What’s your weirdest talent?",
      "Have you ever pretended to like a gift?",
      "Have you ever lied to your parents?",
      "What’s your most embarrassing nickname?",
      "Have you ever failed a subject?",
      "What was your childhood fear?",
      "What’s something you're afraid to try?",
      "Have you ever skipped school?",
      "Have you ever been caught cheating?",
      "What’s the longest you've gone without showering?",
      "What’s your biggest regret?",
      "Have you ever cried in public?",
      "Do you sleep with stuffed toys?",
      "What’s your favorite guilty snack?",
      "Have you ever laughed at the wrong moment?",
      "What’s something you’ve done you’ll never do again?",
      "What’s your weirdest phobia?",
      "If you could change one thing about your body, what would it be?",
      "Have you ever stalked someone online?",
      "Have you ever said 'I love you' and not meant it?",
      "What’s your most used emoji?",
      "Do you believe in ghosts?",
      "If you had to be someone else for a day, who?",
      "Have you ever sent the wrong message to someone?",
      "What’s your weirdest food combo?",
      "What lie do you use most often?",
      "What’s your worst haircut ever?",
      "If animals could talk, what would your pet say about you?"
    ];

    const dares = [
      "Do 10 jumping jacks.",
      "Sing the chorus of your favorite song.",
      "Dance for 30 seconds with no music.",
      "Act like a monkey for 1 minute.",
      "Spin around 5 times and walk in a straight line.",
      "Speak in an accent for the next round.",
      "Say the alphabet backwards.",
      "Do your best evil laugh.",
      "Do a runway walk.",
      "Tell a joke that's so bad it's good.",
      "Make a silly face for 30 seconds.",
      "Let someone draw on your hand.",
      "Do 5 different animal sounds.",
      "Say a tongue twister 5 times fast.",
      "Balance a spoon on your nose.",
      "Try to lick your elbow.",
      "Wear your socks on your hands for 2 minutes.",
      "Let someone give you a new hairstyle.",
      "Try to do a handstand (be safe!).",
      "Act like a robot for 1 minute.",
      "Pretend to be a cat for 30 seconds.",
      "Do 15 squats.",
      "Draw a mustache on your face.",
      "Imitate your favorite actor.",
      "Speak only in rhymes for 2 rounds.",
      "Post a silly photo on social media.",
      "Do 20 sit-ups.",
      "Let someone tickle you for 15 seconds.",
      "Say something nice about every player.",
      "Make up a poem about the player on your left.",
      "Act like a superhero for 30 seconds.",
      "Do your best impression of a famous singer.",
      "Pretend your shoe is a phone and talk to it.",
      "Make a funny sound every time you blink for 1 minute.",
      "Draw with your non-dominant hand.",
      "Tell a dramatic story about brushing your teeth.",
      "Eat a small spoon of ketchup or mustard.",
      "Balance on one foot for 20 seconds.",
      "Let someone write something funny on your arm.",
      "Pretend to cry for 10 seconds.",
      "Say 'I’m a genius' in the weirdest voice possible.",
      "Create a handshake with the person on your right.",
      "Do a crab walk across the room.",
      "Hop like a bunny in a circle.",
      "Pretend to be a dog for 1 minute.",
      "Wear a funny hat made of tissue.",
      "Do your best chicken dance.",
      "Recite a poem with dramatic emotion.",
      "Do a cartwheel or attempt one safely.",
      "Make up a new TikTok dance move."
    ];

    const situations = [
      "Everyone gives you a truth to answer.",
      "Everyone dares you to do one mini task.",
      "Switch seats with the player opposite you.",
      "You choose someone to swap challenges with.",
      "You get to skip your turn!",
      "Everyone else gets a free spin.",
      "You must complete both a truth AND a dare.",
      "Choose one player to do your next challenge.",
      "You're the judge for the next 2 turns.",
      "Everyone must compliment you."
    ];

    function spinWheel() {
      const wheel = document.getElementById("wheel");
      const result = document.getElementById("result");
      const face = document.getElementById("face");

      const rotation = Math.floor(Math.random() * 360) + 720;
      wheel.style.transform = `rotate(${rotation}deg)`;

      setTimeout(() => {
        const chance = Math.random();
        let output = "", emoji = "";

        if (chance < 0.05) {
          // Rare Situation
          output = "🌀 SITUATION:\n" + situations[Math.floor(Math.random() * situations.length)];
          emoji = "🎭";
        } else if (chance < 0.525) {
          // Truth (47.5%)
          output = "💬 TRUTH:\n" + truths[Math.floor(Math.random() * truths.length)];
          emoji = "🙂";
        } else {
          // Dare (47.5%)
          output = "🔥 DARE:\n" + dares[Math.floor(Math.random() * dares.length)];
          emoji = "😈";
        }

        result.innerText = output;
        face.innerText = emoji;
      }, 3000); // Wait for spin to end
    }
  </script>
</body>
</html>
