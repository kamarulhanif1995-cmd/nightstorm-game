
StormNight/
├── game/
│   ├── script.rpy
│   └── images/ (folder, leave empty for now)
└── options.rpy

# nightstorm-game# Kylerul: Storm Night - FULL GAME + ADULT SCENES + MANIPULATION PATH
# Paste this entire block into your game/script.rpy

define k = Character("Kylerul", color="#2a2a2a")
define s = Character("Sara", color="#e8c97a")
define n = Character("Nia", color="#c86bff")
define sh = Character("Shasha", color="#c0d8ff")

label start:
    # --- INITIALIZATION ---
    $ sara_trust = 20
    $ nia_trust = 20
    $ shasha_trust = 20
    $ guardian = 0
    $ stigma = 0
    $ manipulation = 0  # NEW STAT: Measures how much you control/manipulate them

    scene black with fade
    "Night 1 - 8 PM - Island - Storm Coming"
    scene bg_storm with fade
    "Sound: Wind howling, rain hitting the roof."

    # ===== NIGHT 1 BLOCK 1 - 8 PM =====
    scene bg_chalet_night with fade
    "Knock. Knock. Knock."
    "Three girls, wet from rain, standing at the door. Their chalet just collapsed from a fallen tree."
    s "Th-thank you... our place... the tree..."
    n "Dude, you actually exist! I thought you were a myth."
    sh "Kylerul, thank you for the shelter. We... we had nowhere else to go."
    k "Come in. You're safe now."
    
    menu:
        "What do you do first?"
        "Give Sara your jacket - She is shivering":
            $ sara_trust += 8
            $ nia_trust += 2
            k "Take it. You're cold."
            s "Oh... thank you... it's warm..."
            n "Okay, softie confirmed."
        "Give everyone blankets first - No favoritism":
            $ sara_trust += 4
            $ nia_trust += 4
            $ shasha_trust += 4
            $ guardian += 2
            k "Everyone. Sit near the light. Blankets here."
            sh "Thank you. That's... kind."
            s "It's warm in here..."
        "Tell them to sit and be quiet - Storm is bad":
            $ sara_trust -= 5
            $ nia_trust -= 5
            $ shasha_trust -= 5
            $ stigma += 2
            $ manipulation += 3  # Controlling behavior increases manipulation
            k "Don't panic. Sit. Don't waste the candle."
            n "...Okay, boss."
            s "...Okay."
    "They sit down. Candle flickers. Storm outside."

    # ===== NIGHT 1 BLOCK 2 - 10 PM FREE ROAM =====
    "10 PM - They need to settle. You can only go to ONE place."
    menu:
        "Where do you go at 10 PM?"
        "Kitchen - Help Sara find water":
            scene bg_kitchen with fade
            s "I... I was trying to find water. I'm good at lists, not... finding things."
            menu:
                "Help her":
                    $ sara_trust += 7
                    k "You lead, I'll carry. You're smarter than me at this."
                    s "Really? No one says that to me... Thank you."
                "Let her do it alone":
                    $ sara_trust -= 4
                    $ manipulation += 2
                    k "You'll figure it out."
                    s "...Okay. I'll try."
        "Porch - Check on Nia watching the storm":
            scene bg_porch with fade
            n "Storm is kinda beautiful, right? If you ignore the part where it tried to kill us."
            menu:
                "Stand beside her quietly - No forced talk":
                    $ nia_trust += 7
                    k "..."
                    n "Huh. You just... stand here? No lecture?"
                    n "I like that. Thanks."
                "Drag her inside - It's dangerous":
                    $ nia_trust -= 5
                    $ manipulation += 4
                    k "Get inside. It's dangerous."
                    n "I know! I was just looking! God."
        "Bedroom Corner - Bring towel to Shasha":
            scene bg_bedroom with fade
            sh "I'm fine. I just need a moment to fix my hair. I must look... a mess."
            menu:
                "Give towel - You don't have to be perfect now":
                    $ shasha_trust += 8
                    k "You don't have to be perfect now. Just dry off."
                    sh "...No one has ever said that to me."
                    sh "Thank you."
                "Tell her to rest, she looks tired":
                    $ shasha_trust += 3
                    $ manipulation += 2
                    k "You should rest. You look tired."
                    sh "I... I am tired. But I shouldn't show it."

    # ===== NIGHT 1 BLOCK 3 - 12 AM STORM PEAK =====
    scene bg_living with fade
    "12 AM - Thunder crashes. Lights flicker. Everyone jumps."
    s "A-ah!"
    n "Okay! That one was close!"
    sh "I... I am fine. I am fine."
    menu:
        "What do you do at Storm Peak?"
        "Stay calm, speak low - Guardian":
            $ sara_trust += 5
            $ nia_trust += 5
            $ shasha_trust += 5
            $ guardian += 2
            k "It's just thunder. We're safe. Look at the candle, not the window."
            s "Okay... looking at candle..."
            n "Wow. Your voice... okay, I'm calm now."
            sh "Thank you. I needed that."
        "Use calm commanding voice - Stigma":
            $ sara_trust += 2
            $ nia_trust += 2
            $ shasha_trust += 2
            $ stigma += 2
            $ manipulation += 3  # Commanding is manipulative
            k "Listen to me. Close your eyes. Sleep now."
            s "...Okay..."
            n "...Okay, that's kinda scary but it works."
            sh "...Yes."
        "Stay silent":
            $ sara_trust -= 6
            $ nia_trust -= 6
            $ shasha_trust -= 6
            $ manipulation += 4
            k "..."
            s "Kylerul? Are you... there?"
            n "Great. Ghost mode activated."

    # ===== NIGHT 1 BLOCK 4 - 2 AM QUIET (ADULT / MANIPULATION SCENE TRIGGER) =====
    "2 AM - Storm calms a little. Everyone is tired. Honest talk unlocks."
    
    # Determine who gets the scene based on Highest Trust
    # But modify the tone based on Manipulation
    
    current_leader = ""
    if sara_trust >= nia_trust and sara_trust >= shasha_trust:
        current_leader = "sara"
    elif nia_trust >= sara_trust and nia_trust >= shasha_trust:
        current_leader = "nia"
    else:
        current_leader = "shasha"

    # If no one has high trust, skip to next day
    if current_leader == "" or max(sara_trust, nia_trust, shasha_trust) < 40:
        jump night1_skip_intimacy

    # Decide Scene Tone based on Manipulation
    # High Manipulation (>10) makes the scene more possessive/dark
    # Low Manipulation makes it sweet/vulnerable
    
    if manipulation > 10:
        $ tone = "dark"
    else:
        $ tone = "sweet"

    if current_leader == "sara":
        if tone == "dark":
            jump night1_sara_dark
        else:
            jump night1_sara_sweet
    elif current_leader == "nia":
        if tone == "dark":
            jump night1_nia_dark
        else:
            jump night1_nia_sweet
    else: # Shasha
        if tone == "dark":
            jump night1_shasha_dark
        else:
            jump night1_shasha_sweet

label night1_skip_intimacy:
    scene bg_living with fade
    "The storm fades into a gentle rain. Everyone falls asleep quickly."
    jump day2_start

# ==========================================
# SARA SCENES
# ==========================================

label night1_sara_sweet:
    scene bg_bedroom with fade
    "Sara is sitting on the edge of the bed, shivering slightly despite the blanket."
    s "Kylerul... can I ask you something?"
    s "I'm brilliant in exams, but dumb in real life. People laugh at me when I stutter."
    s "You didn't laugh. You waited. Why?"
    menu:
        "You deserve to finish talking":
            $ sara_trust += 10
            k "You deserve to finish talking. That's it."
            s "I... I want to talk to you again. After this."
        "It was nothing":
            $ sara_trust -= 3
            k "It was nothing."
            s "Oh... okay."
    
    "She looks up at you, eyes wide and vulnerable."
    s "Can I... stay close? Just for tonight? I feel safer when you're near."
    menu:
        "Pull her into your arms":
            $ sara_trust += 15
            k "Come here."
            "You pull her close. She fits perfectly against your chest."
            "Her breathing slows down. She smells like vanilla and rain."
            s "Mmm... you're so warm..."
            "She kisses your neck softly."
            s "Thank you, Kylerul. For seeing me."
        "Let her come to you":
            $ sara_trust += 10
            k "Sure. Come here."
            "She crawls onto the bed next to you, curling up like a cat."
            s "Goodnight, Kylerul."
    jump day2_start

label night1_sara_dark:
    scene bg_bedroom with fade
    "Sara is sitting on the edge of the bed, shivering slightly."
    s "Kylerul... can I ask you something?"
    s "I'm brilliant in exams, but dumb in real life."
    s "You didn't laugh. You waited. Why?"
    menu:
        "Because I told you to":
            $ sara_trust += 5
            $ manipulation += 5
            k "Because I decided you were worth listening to."
            s "...That sounds arrogant. But... I like it."
        "Because you're mine now":
            $ sara_trust += 5
            $ manipulation += 10
            k "Because you're mine now. I protect what's mine."
            s "Yours...?"
            s "Does that mean I don't have to worry about anything anymore?"
            k "Exactly. You just listen to me."
            s "Then... I'll listen. Always."
    
    "She looks up at you, eyes wide, a mix of fear and adoration."
    s "Do I... do I belong to you now? Because of the storm?"
    menu:
        "Yes. You belong to me":
            $ sara_trust += 10
            $ manipulation += 10
            k "Yes. You belong to me. Under my roof, under my protection."
            "You grab her chin gently, forcing her to look at you."
            "She blushes deeply, trembling not from cold, but from submission."
            s "I... I understand. I'm yours."
            "She kisses your hand, then climbs into your lap."
            "She lets you undress her slowly, her eyes never leaving yours."
            "She takes what you give her, eager to please."
        "Not yet. Earn it":
            $ sara_trust += 5
            k "Not yet. Prove you can handle being mine."
            s "I will. I promise."
    jump day2_start

label night1_nia_sweet:
    scene bg_beach with fade
    "Nia is leaning against the railing, looking at the moon."
    n "Storm is kinda beautiful, right?"
    menu:
        "Join her":
            $ nia_trust += 10
            k "It is. When you ignore the danger."
            n "Yeah. You're different. You don't try to fix everything."
            "She leans her head on your shoulder."
            n "Thanks for letting me breathe."
            "She kisses your cheek softly."
            n "You're the best, Kylerul."
    jump day2_start

label night1_nia_dark:
    scene bg_beach with fade
    "Nia is leaning against the railing, looking wild and untamed."
    n "Storm is kinda beautiful, right?"
    menu:
        "Cage her":
            $ nia_trust += 5
            $ manipulation += 10
            k "Beautiful. Like a beast waiting to be tamed."
            n "Tamed? Me?"
            k "You're loud. Chaotic. But I can hear you clearly over the noise."
            "You step into her space, backing her against the railing."
            n "Hey! Watch it!"
            k "Quiet. Or I'll make you quiet."
            n "You wouldn't dare."
            k "Try me."
            "She holds your gaze, then bites her lip, a spark of adrenaline in her eyes."
            n "Fine. Tame me."
            "She grabs your shirt and pulls you in, kissing you fiercely."
            "It's less about love and more about claiming territory."
            "She rides you hard, demanding you keep up with her energy."
    jump day2_start

label night1_shasha_sweet:
    scene bg_bedroom with fade
    "Shasha is fixing her hair in the mirror, looking exhausted."
    sh "I'm fine. I just need a moment to fix my hair."
    menu:
        "Offer comfort":
            $ shasha_trust += 10
            k "You don't have to be perfect now. Just dry off."
            sh "...No one has ever said that to me."
            sh "Thank you."
            "She turns to you, tears in her eyes."
            sh "Can I... hug you? Just a normal hug? Not the royal wave?"
            "You open your arms. She collapses into them, sobbing softly."
            sh "I'm so tired of being strong."
            "You hold her until she falls asleep in your arms."
    jump day2_start

label night1_shasha_dark:
    scene bg_bedroom with fade
    "Shasha is fixing her hair in the mirror, looking rigid."
    sh "I'm fine. I just need a moment to fix my hair."
    menu:
        "Demand perfection":
            $ shasha_trust += 5
            $ manipulation += 10
            k "Perfection is exhausting, isn't it? That's why you're here."
            sh "Yes... I just want to relax."
            k "Relax by serving. Pour me tea. Fix my collar. Be useful."
            sh "Just... be useful?"
            k "Yes. That's how you earn your sleep."
            sh "Okay. I understand. I will serve you well."
            "She pours tea with trembling hands, careful not to spill a drop."
            "She kneels before you to adjust your shoes."
            sh "Is this... acceptable?"
            k "Acceptable. Now, come here."
            "She climbs onto your lap, offering herself as a prize for obedience."
            "She is stiff at first, then melts, enjoying the release of control."
    jump day2_start

# ==========================================
# DAY 2
# ==========================================

label day2_start:
    scene bg_living with fade
    "Day 2 - 8 AM - Morning After Storm"
    "Island cut off. No boat till tomorrow. No signal, no wifi. Food is low."
    "Only enough food for 3 people, not 4."
    menu:
        "What do you do?"
        "Give your portion to Sara":
            $ sara_trust += 8
            $ shasha_trust -= 2
            k "Eat. You were shivering all night."
        "Split everything into 4 equal small portions":
            $ sara_trust += 4
            $ nia_trust += 4
            $ shasha_trust += 4
            $ guardian += 2
            k "We all eat. Small, but together."
        "Say figure it out yourselves":
            $ sara_trust -= 6
            $ nia_trust -= 6
            $ shasha_trust -= 6
            $ stigma += 3
            $ manipulation += 3
            k "I'm not your dad."

    "10 AM - Free Roam 1 - Pick 1"
    menu:
        "Where do you go at 10 AM?"
        "Kitchen + Sara - Make food list":
            scene bg_kitchen with fade
            s "I can make a list. I'm good at lists."
            menu:
                "You lead, you're smarter than me at this":
                    $ sara_trust += 10
                    k "You lead, you're smarter than me at this."
                    s "Really? Okay... I'll lead."
                "Don't bother, boat comes tomorrow":
                    $ sara_trust -= 5
                    $ manipulation += 2
                    k "Don't bother."
                    s "Oh... okay."
        "Beach + Nia - Go outside":
            scene bg_beach with fade
            n "I need air. I was locked in all night."
            menu:
                "Run with her, no talking, just run":
                    $ nia_trust += 10
                    k "..."
                    n "Yes! This! No lecture, just run!"
                "Stay inside, it's still dangerous":
                    $ nia_trust -= 7
                    $ manipulation += 3
                    k "Stay inside."
                    n "I feel caged."
        "Porch + Shasha - Sit quietly":
            scene bg_porch with fade
            sh "I just need a minute alone... to be not perfect."
            menu:
                "Sit beside her - You don't have to be perfect today":
                    $ shasha_trust += 10
                    k "You don't have to be perfect today."
                    sh "Thank you. I needed to hear that."
                "You should help in kitchen":
                    $ shasha_trust -= 5
                    $ manipulation += 3
                    k "You should help in kitchen."
                    sh "Right. Assistant mode again."

    # 12 PM Ruins
    scene bg_ruins with fade
    "12 PM - Ruins Unlock - You can take ONLY 1 girl back to their destroyed chalet."
    menu:
        "Who do you take to the Ruins?"
        "Take Sara":
            $ sara_trust += 12
            $ nia_trust -= 3
            $ shasha_trust -= 3
            s "My books... they're wet..."
            k "We'll dry them. Together."
            s "Thank you..."
        "Take Nia":
            $ nia_trust += 12
            $ sara_trust -= 3
            $ shasha_trust -= 3
            n "Haha... my stuff is broken... whatever..."
            k "You don't need it. You're you."
            n "...Thanks."
        "Take Shasha":
            $ shasha_trust += 12
            $ sara_trust -= 3
            $ nia_trust -= 3
            sh "I have nothing left..."
            k "You have us."
            sh "...Thank you."
        "Go alone":
            $ sara_trust -= 8
            $ nia_trust -= 8
            $ shasha_trust -= 8
            $ manipulation += 5
            k "I'll go alone."
            "They feel abandoned."

    # 2 PM Free Roam 2
    "2 PM - Free Roam 2 - Pick someone you ignored before for bonus."
    menu:
        "Where do you go at 2 PM?"
        "Kitchen + Sara":
            $ sara_trust += 5
            s "You came back to help me? Thank you."
        "Beach + Nia":
            $ nia_trust += 5
            n "You came to get air with me? Nice."
        "Porch + Shasha":
            $ shasha_trust += 5
            sh "You came to sit with me again? Thank you."

    # 4 PM Food Crisis
    scene bg_living with fade
    "4 PM - Food Crisis - Only one water bottle left."
    menu:
        "What do you do?"
        "Share sip by sip - Guardian":
            $ sara_trust += 6
            $ nia_trust += 6
            $ shasha_trust += 6
            k "Small sips. Everyone."
        "Drink first - Stigma":
            $ sara_trust -= 8
            $ nia_trust -= 8
            $ shasha_trust -= 8
            $ stigma += 2
            $ manipulation += 5
            k "I need strength to protect you. I drink first."
        "Give to lowest Trust girl":
            k "You need it most."
            if sara_trust <= nia_trust and sara_trust <= shasha_trust:
                $ sara_trust += 12
                s "Me? Thank you..."
            elif nia_trust <= sara_trust and nia_trust <= shasha_trust:
                $ nia_trust += 12
                n "Me? ...Thanks."
            else:
                $ shasha_trust += 12
                sh "Me? Thank you..."

    # 6 PM Sunset
    scene bg_sunset with fade
    "6 PM - Sunset - First calm moment in 2 days."
    k "Look. Storm is over. We're still here."
    s "I thought... we wouldn't make it."
    n "Honestly? This is the first time I've felt... calm in a long time."
    sh "I don't want to go back to being perfect. Not yet."
    if (sara_trust + nia_trust + shasha_trust) >= 180:
        "SUNSET CONFESSION UNLOCKED - Each girl says one true thing she never told anyone."
        s "I... I trust you."
        n "I... I feel safe with you."
        sh "I... I want to stay like this."

    # 8 PM Night 2
    scene bg_living with fade
    "8 PM - Night 2 - Second night. No storm, but cold. Only 2 blankets for 4 people."
    menu:
        "What do you do?"
        "Sleep on floor, give blankets to them - Guardian":
            $ sara_trust += 10
            $ nia_trust += 10
            $ shasha_trust += 10
            $ guardian += 2
            k "You three take them. I'll take the floor."
        "Take blanket, make them

 
# This file enables music and audio playback.
define config.has_music = True
define config.has_sound = True
define config.has_transition = True
define config.has_voice = True
 