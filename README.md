<!DOCTYPE html>
<html>

    <head>
        <meta charset='utf-8'>
        <title>Bee Swarm Simulator</title>
    </head>
    <body onload='main()'>
<canvas id='gl-canvas'></canvas>
<canvas id='ui-canvas'></canvas>
<canvas id='tex-canvas' width='2048' height='2048'></canvas>
<style>
    
    body{
        
        overflow:hidden;
        margin:0;
        font-family:comic sans ms;
        user-select:none;
    }
    
    input {
        
        height:24px;
        outline:none;
        background-color:rgb(255,255,255);
        border:none;
        border-bottom:4px solid black;
        transition-duration:0.4s;
        border-radius:5px;
    }
    
    input:hover {
        
        background-color:rgb(200,200,255);
    }
    
    input:focus {
        
        background-color:rgb(170,200,255);
    }
    
    button {
        
        width:60px;
        height:30px;
        transition-duration:0.4s;
        border-radius:8px;
        font-size:16px;
        background-color:rgb(255,255,255);
    }
    
    button:hover {
        
        cursor:pointer;
        border-radius:13px;
        background-color:rgb(150,255,205);
    }
    
    #purchaseButton {
        
        transition-duration:0s;
        border:none;
    }
    
    #purchaseButton:hover {
        
        outline:2px solid rgb(0,0,0,0.4);
    }
    
    #leftShopButton:hover,#rightShopButton:hover{
        
        outline:2px solid rgb(0,0,0,0.4);
    }
    
    #gl-canvas,#tex-canvas{
        
        display:none;
	}
	
	#ui-canvas{
        
        position:fixed;
        left:0;
        top:0;
        z-index:-3;
	}
	
	div::-webkit-scrollbar{
	    
        width:5px;
        height:8px;
        background-color:rgb(0,0,0,0); 
    }
    
    div::-webkit-scrollbar-thumb{
        
        background:rgb(120,120,120);
        border-radius:10px;
    }

    #roboActiveUpgrades::-webkit-scrollbar{
	    
        width:5px;
        height:8px;
        background-color:rgb(0,0,0,0); 
    }
    
    #roboActiveUpgrades::-webkit-scrollbar-thumb{
        
        background:rgb(40,80,40);
        border-radius:10px;
    }
    
    #inventoryButton:hover,#questButton:hover,#beesButton:hover,#settingsButton:hover,#beequipButton:hover{
        
        transition-duration:0.4s;
        background-color:rgba(0,0,0,0.2);
        cursor:pointer;
    }
    
    #feedThisAmount{
        
        margin-top:50px;
        margin-left:5px;
        transition-duration:0s;
        border-radius:6px;
        position:fixed;
        background-color:rgb(0,200,0);
        border:none;
        color:rgb(255,255,255);
        font-size:17px;
        font-family:trebuchet ms;
    }
    
    #feedUntilGifted{
        
        margin-top:100px;
        margin-left:160px;
        transition-duration:0s;
        border-radius:6px;
        position:fixed;
        background-color:rgb(255,200,0);
        border:none;
        color:rgb(255,255,255);
        font-size:16.5px;
        width:135px;
        font-family:trebuchet ms;
    }
    
    #feedThisAmount:hover{
        
        background-color:rgb(0,170,0);
    }
    
    #feedAmount,#blenderCraftAmount,#shrineAmount{
        
        margin-top:50px;
        margin-left:75px;
        transition-duration:0s;
        border-radius:6px;
        position:fixed;
        background-color:rgb(0,30,205);
        border:none;
        color:rgb(255,255,255);
        font-size:17px;
        font-family:arial;
        height:19px;
        width:200px;
        padding:5px;
        padding-left:10px;
    }
    
    #cancelFeeding{
        
        margin-top:100px;
        margin-left:5px;
        transition-duration:0s;
        border-radius:6px;
        position:fixed;
        background-color:rgb(255,30,30);
        border:none;
        color:rgb(255,255,255);
        font-size:17px;
        width:70px;
        font-family:trebuchet ms;
    }
    
    #cancelFeeding:hover{
        
        background-color:rgb(200,0,0);
    }
    
    #actionHoverDarken:hover{
        
        background-color:rgb(0,0,0,0.3);
    }
    
    #blenderX{
        
        background-color:rgb(255,0,0);
        transition-duration:0s;
    }
    
    #blenderX:hover{
        
        background-color:rgb(200,0,0);
    }
    
    #leftBlenderButton,#rightBlenderButton{
        
        background-color:rgb(30,90,255);
    }
    
    #blenderCraft{
        
        background-color:rgb(0,170,0);
        transition-duration:0s;
    }
    
    #blenderCraft:hover{
        
        background-color:rgb(0,100,0);
    }
    
    #leftBlenderButton:hover,#rightBlenderButton:hover{
        
        background-color:rgb(19, 54, 150);
    }
    
    #blenderSpeed,#blenderEnd{
        
        transition-duration:0s;
    }
    
    #shrineX:hover{
        
        background-color:rgb(200,0,0);
    }

    #shrineX{
        
        background-color:rgb(255,0,0);
    }
    
    #shrineDonate{
        
        background-color:rgb(0,190,0);
    }
    
    #shrineDonate:hover{
        
        cursor:pointer;
        background-color:rgb(0,140,0);
    }
    
    #shrineLeft:hover,#shrineRight:hover{
        
        cursor:pointer;
        background:rgb(0,0,0,0.1);
    }
    
</style>

<div id='abilityUI' style='margin:5px;padding:0px;margin-top:35px;position:fixed'>
    
    <svg id='inspireCoconutsPassive' style='width:30px;height:30px;display:none;'>
        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        
        <circle cx='16' cy='14' r='8' fill='rgb(150,75,0)' stroke='rgb(0,0,0)' stroke-width='1'></circle>
        
        <circle cx='18' cy='15' r='1.5' fill='rgb(94, 51, 7)'></circle><circle cx='13' cy='12' r='1.5' fill='rgb(94, 51, 7)'></circle><circle cx='18' cy='11' r='1.5' fill='rgb(94, 51, 7)'></circle>
        
        <path d='M0.00000 3.00000L4.70228 6.47214L2.85317 0.92705L7.60845 -2.47214L1.76336 -2.42705L0.00000 -8.00000L-1.76336 -2.42705L-7.60845 -2.47214L-2.85317 0.92705L-4.70228 6.47214Z' transform='translate(12,19) scale(0.75,0.75)' fill='rgb(255,255,0)' stroke='rgb(200,200,0)'></path>
        
        <rect id='inspireCoconutsPassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='inspireCoconutsPassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>
    
    <svg id='emergencyCoconutShieldPassive' style='width:30px;height:30px;display:none;'>
        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        
        <circle cx='15' cy='15' r='11' fill='rgb(255,255,255,0.5)'></circle>
        <circle cx='15' cy='15' r='8' fill='rgb(150,75,0)' stroke='rgb(0,0,0)' stroke-width='1'></circle>
        
        <circle cx='17' cy='16' r='1.5' fill='rgb(94, 51, 7)'></circle><circle cx='12' cy='13' r='1.5' fill='rgb(94, 51, 7)'></circle><circle cx='17' cy='12' r='1.5' fill='rgb(94, 51, 7)'></circle>
        
        <rect id='emergencyCoconutShieldPassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='emergencyCoconutShieldPassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>

    <svg id='coconutHastePassive' style='width:30px;height:30px;display:none;'>

        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        
        <circle cx='15' cy='15' r='8' fill='rgb(150,75,0)' stroke='rgb(0,0,0)' stroke-width='1'></circle>
        
        <circle cx='17' cy='16' r='1.5' fill='rgb(94, 51, 7)'></circle><circle cx='12' cy='13' r='1.5' fill='rgb(94, 51, 7)'></circle><circle cx='17' cy='12' r='1.5' fill='rgb(94, 51, 7)'></circle>
        
        <g transform='translate(6,3) scale(0.75,0.75)'>
        <path d='M18 9L15 21M12 26L15 21M16 26L15 21M11 11L16 15M23 15L16 15' stroke='rgb(255,255,255)' stroke-width='2'></path>
        <path d='M5 7 L11 7M2 15L12 15M3 23L11 23' stroke='rgb(255,255,255)' stroke-width='1.5' opacity='0.5'></path>
        <circle cx='18' cy='9' r='4'fill='rgb(255,255,255)'></circle></g>

        <rect id='coconutHastePassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='coconutHastePassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>

    
    
    <svg id='xFlamePassive' style='width:30px;height:30px;display:none;'>
        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        
        <path stroke='rgb(255,0,0)' stroke-width='6' d='M8 8L22 22M22 8L8 22'></path>
        <path stroke='rgb(255,100,0)' stroke-width='5' d='M8 8L22 22M22 8L8 22'></path>
        <path stroke='rgb(255,200,0)' stroke-width='1.5' d='M8 8L22 22M22 8L8 22'></path>
        
        <rect id='xFlamePassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='xFlamePassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>
    
    <svg id='ignitePassive' style='width:30px;height:30px;display:none;'>
        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        
        <path d='M15 25C5 24 5 13 15 5M15 25C25 24 25 13 15 5' fill='rgb(255,150,20)'></path>
        <path d='M15 25C10 24 10 17 15 10M15 25C20 24 20 17 15 10' fill='rgb(255,0,0)'></path>
        
        <rect id='ignitePassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='ignitePassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>
    
    <svg id='bubbleBombsPassive' style='width:30px;height:30px;display:none;'>
        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        
        <circle cx='15' cy='18' r='7' fill='rgb(80,200,255)' ></circle>
        <circle cx='15' cy='31' r='3' fill='rgb(80,80,80)'  transform='scale(1,0.4)'></circle>
        <path stroke='rgb(222, 222, 149)' stroke-width='2' fill='rgb(0,0,0,0)' d='M15 12C15 10 15 6 18 8'></path>
        <circle cx='18' cy='7' r='2' fill='rgb(255,100,0)' ></circle>
        
        <rect id='bubbleBombsPassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='bubbleBombsPassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>
    
    <svg id='coinScatterPassive' style='width:30px;height:30px;display:none;'>
        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        
        <circle cx='15' cy='15' r='9' fill='rgb(230, 208, 14)' ></circle>
        
        <path fill='rgb(217, 184, 52)' stroke='rgb(0,0,0)' stroke-width='1' d='M13 10C16 10 24 17 15 20C8 12 18 15 13 10Z'></path>
        <rect id='coinScatterPassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='coinScatterPassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>
    
    <svg id='diamondDrainPassive' style='width:30px;height:30px;display:none;'>
        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        
        <path fill='rgb(100,200,255)' d='M-2 0L2 0L4 2L0 6L-4 2Z' transform='translate(18,9) scale(1.9,2.15)' stroke='rgb(0,0,0)' stroke-width='0.6'></path>
        <path fill='rgb(242, 199, 29)' d='M13 10C16 10 24 17 15 20C8 12 18 15 13 10Z' transform='translate(-8,-4) scale(1.2,1.2)' stroke='rgb(0,0,0)' stroke-width='0.7'></path>
        
        <rect id='diamondDrainPassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='diamondDrainPassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>
    
    <svg id='gummyMorphPassive' style='width:30px;height:30px;display:none;'>
        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        
        <rect x='9' y='10' width='12' height='12' fill='rgb(255,50,255,0.75)'></rect>
        <path fill='rgb(25,255,126,0.75)' d='M9 21 C11 10 14 10 14 19C13 20 13 20 16 19C16 10 19 10 21 21'></path>
        <circle cx='10' cy='10' r='3' fill='rgb(25,255,126,0.75)'></circle><circle cx='20' cy='10' r='3' fill='rgb(25,255,126,0.75)'></circle>
        <circle cx='13' cy='15' r='0.7' fill='rgb(0,0,0)'></circle><circle cx='17' cy='15' r='0.7' fill='rgb(0,0,0)'></circle>
        
        <rect id='gummyMorphPassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='gummyMorphPassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>
    
    <svg id='focusPulserPassive' style='width:30px;height:30px;display:none;'>
        
        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        
        <path fill='rgb(255,255,255)' d='M15 25L7 18L12 15L 8 11C6 6 15 5 19 13L 20 15L13 20Z' transform='translate(2,0)' stroke='rgb(255,0,0)' stroke-width='2'></path>
        
        <rect id='focusPulserPassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='focusPulserPassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>
    
    <svg id='hastePulserPassive' style='width:30px;height:30px;display:none;'>
        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        
        <path fill='rgb(255,255,255)' d='M15 25L7 18L12 15L 8 11C6 6 15 5 19 13L 20 15L13 20Z' transform='translate(2,0)' stroke='rgb(0,0,255)' stroke-width='2'></path>
        
        <rect id='hastePulserPassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='hastePulserPassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>
    
    <svg id='petalStormPassive' style='width:30px;height:30px;display:none'>
        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        
        <g transform='translate(15,15) rotate(48)'>
            <path fill='rgb(255,255,255,0.25)' d='M 0 0C 7 -20 14 5 0 0' ></path>
        <path fill='rgb(255,255,255,0.25)' d='M 0 0C 7 -20 14 5 0 0' transform='rotate(122)'></path>
        <path fill='rgb(255,255,255,0.25)' d='M 0 0C 7 -20 14 5 0 0' transform='rotate(249)'></path>
        </g>
        <g transform='translate(15,15) rotate(26)'>
            <path fill='rgb(255,255,255,0.5)' d='M 0 0C 7 -20 14 5 0 0' ></path>
        <path fill='rgb(255,255,255,0.5)' d='M 0 0C 7 -20 14 5 0 0' transform='rotate(122)'></path>
        <path fill='rgb(255,255,255,0.5)' d='M 0 0C 7 -20 14 5 0 0' transform='rotate(249)'></path>
        </g>
        
        <path fill='rgb(255,255,255)' d='M 0 0C 7 -20 14 5 0 0' transform='translate(15,15)'></path>
        <path fill='rgb(255,255,255)' d='M 0 0C 7 -20 14 5 0 0' transform='translate(15,15) rotate(122)'></path>
        <path fill='rgb(255,255,255)' d='M 0 0C 7 -20 14 5 0 0' transform='translate(15,15) rotate(249)'></path>
        
        <circle fill='rgb(255, 255, 120)' cx='15' cy='15' r='3'></circle>
        
        <rect id='petalStormPassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='petalStormPassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>
    
    <svg id='popStarPassive' style='width:30px;height:30px;display:none;'>
        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        <circle cx='8' cy='8' r='2' fill='rgb(30,110,205)' stroke='rgb(0,0,0,0.4)'></circle><circle cx='22' cy='10' r='3' fill='rgb(30,110,205)' stroke='rgb(0,0,0,0.4)'></circle><circle cx='15' cy='22' r='3' fill='rgb(30,110,205)' stroke='rgb(0,0,0,0.4)'></circle>
        <path d='M0.00000 3.00000L4.70228 6.47214L2.85317 0.92705L7.60845 -2.47214L1.76336 -2.42705L0.00000 -8.00000L-1.76336 -2.42705L-7.60845 -2.47214L-2.85317 0.92705L-4.70228 6.47214Z' transform='translate(15,15) scale(1.3,1.3)' fill='rgb(60,170,255)' stroke='rgb(10,80,200)'></path>
        <rect id='popStarPassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='popStarPassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>
    
    <svg id='scorchingStarPassive' style='width:30px;height:30px;display:none;'>
        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        <path d='M8 12 C5 10 5 8 10 3C15 15 15 15 13 15M19 12C15 15 25 1 25 12Z' fill='rgb(255,100,0)'></path>
        <path d='M0.00000 3.00000L4.70228 6.47214L2.85317 0.92705L7.60845 -2.47214L1.76336 -2.42705L0.00000 -8.00000L-1.76336 -2.42705L-7.60845 -2.47214L-2.85317 0.92705L-4.70228 6.47214Z' transform='translate(15,15) scale(1.3,1.3)' fill='rgb(220,0,0)' stroke='rgb(150,0,0)'></path>
        <rect id='scorchingStarPassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='scorchingStarPassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>
    
    <svg id='gummyStarPassive' style='width:30px;height:30px;display:none;'>
        <defs>
            <linearGradient id='gummyStarGrad' x1='0.4' x2='0.6' y1='0.4' y2='0.6'>
                <stop offset='10%' stop-color='rgb(255, 50, 255)'/>
                <stop offset='90%' stop-color='rgb(25, 255, 126)'/>
            </linearGradient>
            <linearGradient id='gummyStarOutline' x1='0.4' x2='0.6' y1='0.4' y2='0.6'>
                <stop offset='10%' stop-color='rgb(0, 205, 86)'/>
                <stop offset='90%' stop-color='rgb(205, 10, 205)'/>
            </linearGradient>
        </defs>
        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        <circle cx='9' cy='7' fill='rgb(205,10,205)' r='2'></circle>
        <circle cx='20' cy='7' fill='rgb(0,205,86)' r='2'></circle>
        <circle cx='24' cy='19' fill='rgb(205,10,205)' r='2'></circle>
        <circle cx='6' cy='19' fill='rgb(0,205,86)' r='2'></circle>
        <path d='M0.00000 3.00000L4.70228 6.47214L2.85317 0.92705L7.60845 -2.47214L1.76336 -2.42705L0.00000 -8.00000L-1.76336 -2.42705L-7.60845 -2.47214L-2.85317 0.92705L-4.70228 6.47214Z' transform='translate(15,15) scale(1.3,1.3)' fill='url(#gummyStarGrad)' stroke='url(#gummyStarOutline)'></path>
        <rect id='gummyStarPassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='gummyStarPassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>
    
    <svg id='guidingStarPassive' style='width:30px;height:30px;display:none;'>
        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        
        <g transform='scale(0.75,0.75) translate(5,5)'>
        <circle cx='15' cy='15' r='13' fill='rgb(200,200,200)' stroke='rgb(90,90,90)' stroke-width='3'></circle>
        <path d='M0 -5L-2 10L2 10Z' fill='rgb(255,0,0)' transform='translate(15,15) rotate(40) translate(0,-8)'></path>
        <path d='M0 -5L-2 10L2 10Z' fill='rgb(0,0,255)' transform='translate(15,15) rotate(180) translate(0,-8)'></path>
        <path d='M0 -5L-2 10L2 10Z' fill='rgb(0,0,0)' transform='translate(15,15) rotate(-40) translate(0,-8)'></path>
        <path d='M0.00000 3.00000L4.70228 6.47214L2.85317 0.92705L7.60845 -2.47214L1.76336 -2.42705L0.00000 -8.00000L-1.76336 -2.42705L-7.60845 -2.47214L-2.85317 0.92705L-4.70228 6.47214Z' transform='translate(15,15) scale(1.3,1.3)' fill='rgb(255,235,100)' stroke='rgb(100,100,100)'></path>
        
        </g>
        
        <rect id='guidingStarPassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='guidingStarPassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>
    
    <svg id='starShowerPassive' style='width:30px;height:30px;display:none'>
        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        <path d='M 16 13L 20 9M 16 6L 12 10M 19 19L 25 13' stroke='rgb(255, 255, 0,0.5)' stroke-width='2'></path>
        <path d='M0.00000 3.00000L4.70228 6.47214L2.85317 0.92705L7.60845 -2.47214L1.76336 -2.42705L0.00000 -8.00000L-1.76336 -2.42705L-7.60845 -2.47214L-2.85317 0.92705L-4.70228 6.47214Z' transform='translate(12,18) scale(0.8,0.8)' fill='rgb(255,235,100)' stroke='rgb(196, 196, 57)'></path>
        <rect id='starShowerPassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='starShowerPassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>
    
    <svg id='starSawPassive' style='width:30px;height:30px;display:none'>
        <circle cx='15' cy='15' r='14' fill='rgb(115,163,0)' stroke='rgb(0,0,0)' stroke-width='2'></circle>
        
        <circle cx='12' cy='19' r='7' fill='rgb(0,0,0,0)' stroke='rgb(255,255,255,0.5)' stroke-width='3' transform='scale(1.2,0.8)'></circle>
        <path d='M0.00000 3.00000L4.70228 6.47214L2.85317 0.92705L7.60845 -2.47214L1.76336 -2.42705L0.00000 -8.00000L-1.76336 -2.42705L-7.60845 -2.47214L-2.85317 0.92705L-4.70228 6.47214Z' transform='translate(15,18) scale(1.2,0.8)' fill='rgb(255,255,255)' stroke='rgb(150,150,150)'></path>
        <rect id='starSawPassive_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='starSawPassive_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>
    
    <svg id='tabbyLove' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(252, 186, 3)'></rect>
        <rect id='tabbyLove_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0'></rect>
        
        <circle cx='10' cy='13' r='2.2'></circle>
        <circle cx='20' cy='13' r='2.1'></circle>
        
        <path fill='rgb(0,0,0,0)' stroke='rgb(0,0,0)' stroke-width='1.25' d='M12 17C 12 19 18 19 18 17M15 18L15 21M10 21C10 23 15 23 15 21M15 18L15 21M20 21C20 23 15 23 15 21M0 16L8 18M0 23L8 21M30 16L22 18M30 23L22 21'></path>
        
        <path fill='rgb(0,0,0,0)' stroke='rgb(166, 129, 43)' stroke-width='2.5' d='M10 0L10 5M15 0 L 15 6M20 0 L20 5'></path>

        <text id='tabbyLove_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>

    <svg id='scienceEnhancement' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(252, 186, 3)'></rect>
        <rect id='scienceEnhancement_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0'></rect>
        
        <g transform="translate(4,4) scale(0.5,0.5)" fill="#000000">

            <path d="M43.183,37.582L31.209,15.429V2.232C31.209,0.99,30.17,0,28.929,0H16.707c-1.242,0-2.264,0.99-2.264,2.232v13.197 L2.459,37.582c-0.914,1.689-0.868,3.74,0.115,5.391c0.983,1.649,2.766,2.668,4.686,2.668h31.115c1.92,0,3.707-1.019,4.69-2.668 C44.047,41.322,44.097,39.271,43.183,37.582z M24.797,28.314c1.073,0,1.942,0.869,1.942,1.942c0,1.072-0.871,1.942-1.942,1.942 c-1.072,0-1.942-0.87-1.942-1.942C22.855,29.186,23.724,28.314,24.797,28.314z M19.336,16.637c1.073,0,1.942,0.87,1.942,1.943 c0,1.072-0.869,1.942-1.942,-6c-1.073,0-1.942-0.87-1.942-1.942C17.395,17.507,18.263,16.637,19.336,16.637z M 19,23.417 c1.738,0,3.148,1.41,3.148,3.147c0,1.738-1.41,3.147-3.148,3.147c-1.739,0-3.148-1.409-3.148-3.147S17.597,23.417,19.336,23.417z M37.414,39.562c-0.404,0.688-1.143,1.094-1.938,1.094H10.159c-0.796,0-1.534-0.406-1.938-1.094 c-0.404-0.688-0.415-1.528-0.028-2.226l3.043-5.47c0.434-0.782,1.29-1.23,2.18-1.145c4.041,0.385,8.583,3.842,12.642,3.688 c2.174-0.083,4.192-0.875,6.114-1.934c1.085-0.6,2.45-0.207,3.052,0.877l2.219,3.982z" />

        </g>
        
        <text id='scienceEnhancement_amount' x='28' y='28' fill='white' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>

    <svg id='polarPower' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(100,200,255)'></rect>
        <rect id='polarPower_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0'></rect>

        <text id='polarPower_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
        <path fill='rgb(0,0,0,0)' stroke='black' stroke-width='2.5' d='M10 8C8 18 22 18 20 8M15 8 15 25'></path>
    </svg>

    <svg id='comfortingNectar' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(122,153,174)'></rect>
        <rect id='comfortingNectar_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <path fill='rgb(175, 218, 248)' d='M 22 26C -2 39 0 -1 19 8C 13 5 5 25 22 24' transform='translate(5,2) scale(0.8,0.75)'></path>

        <text id='comfortingNectar_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>

    <svg id='invigoratingNectar' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(174,87,79)'></rect>
        
        <rect id='invigoratingNectar_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='scale(0.9,0.9) translate(2,4)'>
        <path fill='rgb(248, 124, 113)' d='M15 25 C 1 26 5 10 8 8L 12 16L15 0L18 16L 22 8C 27 12 28 27 15 25'></path>
        <path stroke='rgb(174,87,79)' stroke-width='3' d='M15 19 L15 30'></path>
        </g>
        <text id='invigoratingNectar_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>

    <svg id='motivatingNectar' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(140,116,165)'></rect>
        <rect id='motivatingNectar_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='translate(1,0) scale(0.95,0.95)'>
        <path fill='rgb(200, 166, 236)' d='M 10 20 C 0 1 30 0 20 20'></path>
        <rect fill='rgb(200, 166, 236)' x='10' y='22' width='10' height='4' rx='2'></rect>
        <path stroke='rgb(140,116,165)' fill='rgb(0,0,0,0)' stroke-width='2' d='M 11 15 L 13 12L 15 15L 17 12L19 15'></path>
        </g>
        
        <text id='motivatingNectar_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>

    <svg id='refreshingNectar' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(117,174,113)'></rect>
        <rect id='refreshingNectar_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='scale(0.9,0.9) translate(2,0)'>
        <path fill='rgb(167, 248, 162)' d='M 15 9 C -6 36 37 34 15 9' transform='scale(1.25,1.25) translate(-3,-5)'></path>
        <path fill='rgb(117,174,113)' d='M 15 9 C -6 36 37 34 15 9' transform='scale(0.3,0.3) translate(36,60)'></path>
        <path fill='rgb(117,174,113)' d='M 15 9 C -6 36 37 34 15 9' transform='scale(0.3,-0.3) translate(36,-72)'></path>
        </g>
        
        <text id='refreshingNectar_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>

    <svg id='satisfyingNectar' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(174,148,162)'></rect>
        <rect id='satisfyingNectar_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <circle cx='15' cy='15' r='10' fill='rgb(248, 211, 231)'></circle>
        <path stroke='rgb(174,148,162)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 19 4 C 4 25 26 3 11 27'></path>
        
        <text id='satisfyingNectar_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>

    <svg id='roboChallengeBuff' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(148, 148, 148,0)'></rect>

        <text id='roboChallengeBuff_amount' x='29' y='29' style='font-family:calibri;font-size:12px' text-anchor='end' fill='rgb(0,0,0)'></text>
        
        <rect id='roboChallengeBuff_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0'></rect>
        
        <g transform='translate(15,15) scale(0.6,0.6)'>
            <path fill='rgb(255, 255, 160)' stroke='black' stroke-width='2' d='M -20 -15L 15 -15L 20 -10L 20 10L 15 15L-20 15L-20 -15'></path>
            <path fill='rgb(0,0,0,0)' stroke='rgb(130,160,130)' stroke-width='4' d='M -17 -14L -17 14M 13 -14L 18 -8L 18 8L 13 14'></path>
            <circle cx='0' cy='0' r='6' fill='rgb(100,150,100)'></circle>
            <circle cx='0' cy='0' r='3' fill='rgb(255, 255, 160)'></circle>
            
            <rect x='-2.5' y='-9' fill='rgb(100,150,100)' width='5' height='4' transform='rotate(0)'></rect>
            <rect x='-2.5' y='-9' fill='rgb(100,150,100)' width='5' height='4' transform='rotate(45)'></rect>
            <rect x='-2.5' y='-9' fill='rgb(100,150,100)' width='5' height='4' transform='rotate(90)'></rect>
            <rect x='-2.5' y='-9' fill='rgb(100,150,100)' width='5' height='4' transform='rotate(135)'></rect>
            <rect x='-2.5' y='-9' fill='rgb(100,150,100)' width='5' height='4' transform='rotate(180)'></rect>
            <rect x='-2.5' y='-9' fill='rgb(100,150,100)' width='5' height='4' transform='rotate(220)'></rect>
            <rect x='-2.5' y='-9' fill='rgb(100,150,100)' width='5' height='4' transform='rotate(265)'></rect>
            <rect x='-2.5' y='-9' fill='rgb(100,150,100)' width='5' height='4' transform='rotate(310)'></rect>
        </g>
        
    </svg>
    
    <svg id='redDriveBuff' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(148, 148, 148,0)'></rect>

        <text id='redDriveBuff_amount' x='29' y='29' style='font-family:calibri;font-size:12px' text-anchor='end' fill='rgb(0,0,0)'></text>
        
        <rect id='redDriveBuff_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0'></rect>
        
        <g transform='translate(15,13) scale(0.5,0.5)'>
        <path fill='rgb(100,100,100)' stroke='black' stroke-width='2' d='M -20 0L0 -15L20 0L0 15z'></path>
        <path fill='rgb(85,85,85)' stroke='black' stroke-width='2' d='M0 25L-20 10L-20 0L0 15z'></path>
        <path fill='rgb(70,70,70)' stroke='black' stroke-width='2' d='M0 25L20 10L20 0L0 15z'></path>
        
        <circle cx='0' cy='0' r='8' stroke='rgb(155,0,0)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        <circle cx='0' cy='-5' r='8' stroke='rgb(255,0,0)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        
        <rect x='-19' y='8' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        <rect x='-13' y='11' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        <rect x='-7' y='14' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        
        </g>
    </svg>
    
    <svg id='whiteDriveBuff' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(148, 148, 148,0)'></rect>

        <text id='whiteDriveBuff_amount' x='29' y='29' style='font-family:calibri;font-size:12px' text-anchor='end' fill='rgb(0,0,0)'></text>
        
        <rect id='whiteDriveBuff_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0'></rect>
        
        <g transform='translate(15,13) scale(0.5,0.5)'>
        <path fill='rgb(100,100,100)' stroke='black' stroke-width='2' d='M -20 0L0 -15L20 0L0 15z'></path>
        <path fill='rgb(85,85,85)' stroke='black' stroke-width='2' d='M0 25L-20 10L-20 0L0 15z'></path>
        <path fill='rgb(70,70,70)' stroke='black' stroke-width='2' d='M0 25L20 10L20 0L0 15z'></path>
        
        <circle cx='0' cy='0' r='8' stroke='rgb(175,175,175)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        <circle cx='0' cy='-5' r='8' stroke='rgb(255,255,255)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        
        <rect x='-19' y='8' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        <rect x='-13' y='11' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        <rect x='-7' y='14' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        
        </g>
    </svg>
    
    <svg id='blueDriveBuff' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(148, 148, 148,0)'></rect>

        <text id='blueDriveBuff_amount' x='29' y='29' style='font-family:calibri;font-size:12px' text-anchor='end' fill='rgb(0,0,0)'></text>
        
        <rect id='blueDriveBuff_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0'></rect>
        
        <g transform='translate(15,13) scale(0.5,0.5)'>
        <path fill='rgb(100,100,100)' stroke='black' stroke-width='2' d='M -20 0L0 -15L20 0L0 15z'></path>
        <path fill='rgb(85,85,85)' stroke='black' stroke-width='2' d='M0 25L-20 10L-20 0L0 15z'></path>
        <path fill='rgb(70,70,70)' stroke='black' stroke-width='2' d='M0 25L20 10L20 0L0 15z'></path>
        
        <circle cx='0' cy='0' r='8' stroke='rgb(0,0,155)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        <circle cx='0' cy='-5' r='8' stroke='rgb(0,0,255)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        
        <rect x='-19' y='8' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        <rect x='-13' y='11' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        <rect x='-7' y='14' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        </g>
    </svg>
    
    <svg id='glitchedDriveBuff' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(148, 148, 148,0)'></rect>

        <text id='glitchedDriveBuff_amount' x='29' y='29' style='font-family:calibri;font-size:12px' text-anchor='end' fill='rgb(0,0,0)'></text>
        
        <rect id='glitchedDriveBuff_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0'></rect>
        
        <g transform='translate(15,13) scale(0.5,0.5)'>
        <path fill='rgb(100,100,100)' stroke='black' stroke-width='2' d='M -20 0L0 -15L20 0L0 15z'></path>
        <path fill='rgb(85,85,85)' stroke='black' stroke-width='2' d='M0 25L-20 10L-20 0L0 15z'></path>
        <path fill='rgb(70,70,70)' stroke='black' stroke-width='2' d='M0 25L20 10L20 0L0 15z'></path>
        
        <g transform='translate(-3,2) scale(0.9,0.9)'>
        <circle cx='0' cy='0' r='8' stroke='rgb(195,195,0,0.5)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        <circle cx='0' cy='-5' r='8' stroke='rgb(255,255,0,0.5)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.5)'></circle>
        </g>
        <g transform='translate(3,2) scale(0.9,0.9)'>
        <circle cx='0' cy='0' r='8' stroke='rgb(189, 100, 180,0.6)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        <circle cx='0' cy='-5' r='8' stroke='rgb(247, 131, 235,0.6)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        </g>
        <g transform='translate(0,-2) scale(0.9,0.9)'>
        <circle cx='0' cy='0' r='8' stroke='rgb(0, 190, 190,0.4)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        <circle cx='0' cy='-5' r='8' stroke='rgb(247, 255, 255,0.4)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        </g>
        
        <rect x='-19' y='8' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        <rect x='-13' y='11' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        <rect x='-7' y='14' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        
        </g>
    </svg>

    <svg id='antChallenge' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(148, 148, 148,0)'></rect>

        <text id='antChallenge_amount' x='29' y='29' style='font-family:calibri;font-size:12px' text-anchor='end' fill='rgb(0,0,0)'></text>
        
        <rect id='antChallenge_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0'></rect>
        
        <g transform='translate(15,15) scale(0.6,0.6)'>
        <path fill='rgb(255, 255, 160)' stroke='black' stroke-width='2' d='M -20 -15L 15 -15L 20 -10L 20 10L 15 15L-20 15L-20 -15'></path>
        <path fill='rgb(0,0,0,0)' stroke='rgb(0,170,0)' stroke-width='4' d='M -17 -14L -17 14M 13 -14L 18 -8L 18 8L 13 14M -8 -4L7 -4L4 -8L-4 -8L-8 -2'></path>
        <path fill='rgb(50,50,50)' d='M -8 -4L8 -4L8 8L-8 8'></path>
        <path stroke='rgb(50,50,50)' fill='rgb(0,0,0,0)' stroke-width='2' d='M -3 -5L-7 -10M 3 -5L7 -10'></path>
        <circle cx='-4' cy='0' r='2' fill='rgb(255,40,40)'></circle>
        <circle cx='4' cy='0' r='2' fill='rgb(255,40,40)'></circle>
        
        </g>
    </svg>

    <svg id='dandelionFieldBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(217, 194, 60)'></rect>
        <rect id='dandelionFieldBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <circle cx='13' cy='11' r='5' fill='rgb(240,240,240)' stroke='#57b5b1' stroke-width='2'></circle>
        <path d='M16 14C 19 19 19 22 17.5 26.5' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='5'></path>
        <path d='M16 14C 19 19 19 22 18 25' fill='rgb(0,0,0,0)' stroke='rgb(0,170,0)' stroke-width='2'></path>
        
        <text id='dandelionFieldBoost_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='sunflowerFieldBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(217, 194, 60)'></rect>
        <rect id='sunflowerFieldBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='translate(5,5)scale(0.7,0.7)'>
        <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(255,255,0)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15)'></path>
        <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(255,255,0)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(45)'></path>
        <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(255,255,0)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(90)'></path>
        <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(255,255,0)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(135)'></path>
        <circle cx='15' cy='15' r='5' fill='rgb(145, 101, 29)'></circle>
        </g>
        <text id='sunflowerFieldBoost_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='blueFlowerFieldBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(217, 194, 60)'></rect>
        <rect id='blueFlowerFieldBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='translate(5,5)scale(0.7,0.7)'>
        <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15)'></path>
        <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(60)'></path>
        <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(120)'></path>
        <circle cx='15' cy='15' r='5' fill='rgb(255,255,0)'></circle>
        </g>
        <text id='blueFlowerFieldBoost_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='mushroomFieldBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(217, 194, 60)'></rect>
        <rect id='mushroomFieldBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <path d='M11 17C 2 18 2 4 15 5M19 17C 28 18 29 4 15 5' fill='rgb(255,40,60)' stroke='#57b5b1' stroke-width='2'></path>
        <path d='M15 18C 2 18 2 4 15 5M15 18C 28 18 29 4 15 5' fill='rgb(255,40,60)'></path>
        <path d='M11 17C 8 30 22 30 19 17' fill='rgb(240,240,240)' stroke='#57b5b1' stroke-width='2'></path>
        <circle cx='11' cy='9' r='1.5' fill='rgb(240,240,240)'></circle>
        <circle cx='26' cy='10' r='2' fill='rgb(240,240,240)' transform='scale(0.8,1)'></circle>
        <circle cx='15' cy='19' r='1.75' fill='rgb(240,240,240)' transform='scale(1,0.7)'></circle>
        
        <text id='mushroomFieldBoost_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='cloverFieldBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(217, 194, 60)'></rect>
        <rect id='cloverFieldBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='translate(4,-4) rotate(10)'>
        <path d='M 18 16C 13 19 16 22 20.3 26' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='5'></path>
        <path d='M 18 16C 13 19 16 22 19 25' fill='rgb(0,0,0,0)' stroke='rgb(0,170,0)' stroke-width='2'></path>
        <g transform='rotate(24)'>
        <circle cx='0' cy='0' r='5' fill='rgb(150,220,150)' stroke='#57b5b1' stroke-width='2' transform='translate(13,8) scale(1,0.6)'></circle><circle cx='0' cy='0' r='1.5' fill='rgb(0,150,0)' transform='translate(13,8) scale(1,0.3)'></circle></g>
        <g transform='rotate(24)'>
        <circle cx='0' cy='0' r='5' fill='rgb(150,220,150)' stroke='#57b5b1' stroke-width='2' transform='translate(24,8) scale(1,0.6)'></circle><circle cx='0' cy='0' r='1.5' fill='rgb(0,150,0)' transform='translate(24,8) scale(1,0.3)'></circle></g>
        <g transform='rotate(114)'>
        <circle cx='0' cy='0' r='5' fill='rgb(150,220,150)' stroke='#57b5b1' stroke-width='2' transform='translate(13,-18) scale(1,0.6)'></circle><circle cx='0' cy='0' r='1.5' fill='rgb(0,150,0)' transform='translate(13,-18) scale(1,0.3)'></circle></g>
        <g transform='rotate(114)'>
        <circle cx='0' cy='0' r='5' fill='rgb(150,220,150)' stroke='#57b5b1' stroke-width='2' transform='translate(3,-18) scale(1,0.6)'></circle><circle cx='0' cy='0' r='1.5' fill='rgb(0,150,0)' transform='translate(3,-18) scale(1,0.3)'></circle></g>
        </g>
        
        <text id='cloverFieldBoost_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='strawberryFieldBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(217, 194, 60)'></rect>
        <rect id='strawberryFieldBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='scale(0.85,0.85) translate(4,5)'>
        <path d='M 13 24C 2 12 2 4 15 5M15 5C 29 5 22 18 12 24' fill='rgb(255,40,60)' stroke='#57b5b1' stroke-width='2'></path>
        
        <g transform='rotate(62) scale(0.7,1) translate(1,-14)'>
        <path d='M 6.8 4C 13 8 16 5 19 1M 15 -2C 13 8 13 5 14 10' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='4'></path>
        <path d='M 8 5C 13 8 16 5 18 2M 15 -1C 13 8 13 5 14 9' fill='rgb(0,0,0,0)' stroke='rgb(0,170,0)' stroke-width='2'></path>
        </g>
        
        <circle cx='10' cy='12' r='1' fill='rgb(255,255,40)'></circle><circle cx='18' cy='10' r='1' fill='rgb(255,255,40)'></circle><circle cx='13' cy='19' r='1' fill='rgb(255,255,40)'></circle><circle cx='15' cy='14' r='1' fill='rgb(255,255,40)'></circle>
        </g>
        
        <text id='strawberryFieldBoost_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='spiderFieldBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(217, 194, 60)'></rect>
        <rect id='spiderFieldBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='scale(0.9,0.9) rotate(-10) translate(-1,3)'>
        <path d='M 15 12C 8 11 10 11 7 10M 13 14C 8 16 7 14 5 14M 13 18C 8 19 7 19 4 18M 13 21C 11 23 7 22 11 24' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='2.75'></path>
        <path d='M 15 12C 8 11 10 11 8 10M 13 14C 8 16 7 14 6 14M 13 18C 8 19 7 19 5 18M 13 21C 11 23 7 22 10 24' fill='rgb(0,0,0,0)' stroke='rgb(70,70,70)' stroke-width='1'></path>
        
        <g transform='scale(-1,1) translate(-30,0)'>
        <path d='M 15 12C 8 11 10 11 7 10M 13 14C 8 16 7 14 5 14M 13 18C 8 19 7 19 4 18M 13 21C 11 23 7 22 10 24' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='2.75'></path>
        <path d='M 15 12C 8 11 10 11 8 10M 13 14C 8 16 7 14 6 14M 13 18C 8 19 7 19 5 18M 13 21C 11 23 7 22 11 24' fill='rgb(0,0,0,0)' stroke='rgb(70,70,70)' stroke-width='1'></path></g>
        
        <circle cx='18.85' cy='18' r='4.5' fill='rgb(70,70,70)' transform='scale(0.8,1)' stroke='#57b5b1' stroke-width='1.5'></circle>
        <circle cx='15' cy='12' r='3' fill='rgb(70,70,70)' stroke='#57b5b1' stroke-width='1.5'></circle>
        <circle cx='15' cy='15' r='1.9' fill='rgb(70,70,70)'></circle></g>
        
        <text id='spiderFieldBoost_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='bambooFieldBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(217, 194, 60)'></rect>
        <rect id='bambooFieldBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <path d='M 13 4.5L12 23.5' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='6'></path>
        <path d='M 13 7L12 22' fill='rgb(0,0,0,0)' stroke='rgb(120,200,70)' stroke-width='3'></path>
        <path d='M 11 11L 14.5 9M 11 15L 14.2 13M 10.5 20L 14 18' fill='rgb(0,0,0,0)' stroke='rgb(70,150,80)' stroke-width='1'></path>
        <circle cx='13' cy='9' r='1.6' fill='rgb(70,150,80)' transform='scale(1,0.7)'></circle>
        
        <g transform='scale(0.75,0.75) translate(12.5,8) rotate(5)'>
        <path d='M 13 4.5L12 23.5' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='6'></path>
        <path d='M 13 7L12 22' fill='rgb(0,0,0,0)' stroke='rgb(120,200,70)' stroke-width='3'></path>
        <path d='M 11 11L 14.5 9M 11 15L 14.2 13M 10.5 20L 14 18' fill='rgb(0,0,0,0)' stroke='rgb(70,150,80)' stroke-width='1'></path>
        <circle cx='13' cy='9' r='1.6' fill='rgb(70,150,80)' transform='scale(1,0.7)'></circle>
        </g>
        
        
        <text id='bambooFieldBoost_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='pineapplePatchBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(217, 194, 60)'></rect>
        <rect id='pineapplePatchBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='translate(-1,3) rotate(-4)'>
        <path d='M 10.5 5.5L15 9M 20.5 5.5L15 9M 14 1L15 9M 17.5 2L15 9' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='3'></path>
        <path d='M 11 6L15 9M 20 6L15 9M 14 2L15 9M 17 3L15 9' fill='rgb(0,0,0,0)' stroke='rgb(0,220,00)' stroke-width='1.5'></path>
        <circle cx='15' cy='13' r='5.5' fill='rgb(255,220,0)' transform='scale(1,1.25)' stroke='#57b5b1' stroke-width='1'></circle>
        <path d='M 10.5 13C 13 15 17 15 19.5 13M 10 18C 13 20.5 17 20.5 20 18M 13 12C 13 12 15 18 13 21M 16 12C 17 13 18 18 16 21' fill='rgb(0,0,0,0)' stroke='rgb(212, 177, 49)' stroke-width='1'></path>
        </g>
        
        <text id='pineapplePatchBoost_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='stumpFieldBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(217, 194, 60)'></rect>
        <rect id='stumpFieldBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='scale(1,0.9) scale(0.93,0.93) rotate(-7) translate(-1,5)'>
        <path d='M 8 9L 8 15 L 4 22L 10 18L 13 23L 18 18L 25 22L22 15L22 9C 22 4 8 4 8 9' fill='rgb(184, 116, 39)' stroke='#57b5b1' stroke-width='1'></path>
        <circle cx='15' cy='18' r='5' fill='rgb(245, 223, 101)' transform='scale(1,0.5)'></circle>
        <circle cx='15' cy='18' r='4' fill='rgb(207, 148, 54)' transform='scale(1,0.5)'></circle>
        <circle cx='15' cy='18' r='2.5' fill='rgb(245, 223, 101)' transform='scale(1,0.5)'></circle>
        <circle cx='15' cy='18' r='1' fill='rgb(207, 148, 54)' transform='scale(1,0.5)'></circle></g>
        
        <text id='stumpFieldBoost_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='cactusFieldBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(217, 194, 60)'></rect>
        <rect id='cactusFieldBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <path d='M 13 18L7 18L 7 11M 16 15L 22 15L 22 7' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='3'></path>
        
        <circle cx='30' cy='12' r='8' fill='rgb(0,230,50)' transform='scale(0.5,1.2)' stroke='#57b5b1' stroke-width='1'></circle>
        
        <path d='M 13 18L7 18L 7 12M 16 15L 22 15L 22 8' fill='rgb(0,0,0,0)' stroke='rgb(0,230,50)' stroke-width='2'></path>
        
        <path d='M 17 21L 19 20M 12 19L 11 21M 7 19L 7 21M 8 12L 9 10M 20 14L 21 12M 15 17L 14 15M 14 9L 12 10M 17 7L 16 5' fill='rgb(0,0,0,0)' stroke='rgb(255,255,200)' stroke-width='1'></path>
        
        <text id='cactusFieldBoost_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='pumpkinPatchBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(217, 194, 60)'></rect>
        <rect id='pumpkinPatchBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <path d='M 15 10C 15 7 16 6 16.75 5.25' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='3.5'></path>
        
        <circle cx='15' cy='19' r='8' fill='rgb(242, 191, 51)' transform='scale(1,0.9)' stroke='#57b5b1' stroke-width='1'></circle>
        
        <path d='M 15 10C 15 7 16 6 16 6' fill='rgb(0,0,0,0)' stroke='rgb(171, 118, 19)' stroke-width='2'></path>
        
        <path d='M 13 13C 11 13 11 21 13 21M 17 13C 19 13 19 21 17 21' fill='rgb(0,0,0,0)' stroke='rgb(0,0,0,0.2)' stroke-width='1'></path>
        
        <text id='pumpkinPatchBoost_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='pineTreeForestBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(217, 194, 60)'></rect>
        <rect id='pineTreeForestBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='scale(0.9,1) rotate(-5) translate(1,-1)'>
        <path d='M 15 22L 15 28' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='5'></path>
        
        <path d='M 15 22L 15 27' fill='rgb(0,0,0,0)' stroke='rgb(171, 118, 19)' stroke-width='3'></path>
        
        <path d='M 15 7L 11 12L 13 12L 8 18L 12 18L 7 23L15 23' fill='rgb(0,160,0)' stroke='#57b5b1' stroke-width='1'></path>
        <path d='M 15 7L 11 12L 13 12L 8 18L 12 18L 7 23L15 23' fill='rgb(0,160,0)' stroke='#57b5b1' stroke-width='1' transform='translate(-15,0) scale(-1,1) translate(-44.75,0)'></path></g>
        
        <text id='pineTreeForestBoost_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='roseFieldBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(217, 194, 60)'></rect>
        <rect id='roseFieldBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <path d='M 16 16C 17 18 17 22 15.95 25M 11 17L 17 18L 21.5 12' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='3.5'></path>
        
        <circle cx='15' cy='9' r='5' fill='rgb(237, 23, 23)' transform='scale(1,1.2)' stroke='#57b5b1' stroke-width='1'></circle>
        <circle cx='14' cy='10' r='3' fill='rgb(237, 23, 23)' transform='scale(1,0.9)' stroke='rgb(0,0,0,0.2)' stroke-width='0.5'></circle>
        <circle cx='15' cy='9' r='3' fill='rgb(237, 23, 23)' transform='scale(1,0.9)' stroke='rgb(0,0,0,0.2)' stroke-width='0.5'></circle>
        <circle cx='14.5' cy='8.5' r='2' fill='rgb(237, 23, 23)' transform='scale(1,0.9)' stroke='rgb(0,0,0,0.2)' stroke-width='0.5'></circle>
        <circle cx='15.5' cy='9.5' r='1' fill='rgb(237, 23, 23)' transform='scale(1,0.9)' stroke='rgb(0,0,0,0.2)' stroke-width='0.5'></circle>
        
        <path d='M 16 16C 17 18 17 22 16 24M 12 17L 17 18L 21 13' fill='rgb(0,0,0,0)' stroke='rgb(0,140,0)' stroke-width='2'></path>
        
        <text id='roseFieldBoost_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='mountainTopFieldBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(217, 194, 60)'></rect>
        <rect id='mountainTopFieldBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='rotate(-10) scale(1.2,1.2) translate(-4,-1)'>
        <path d='M 6 20L 11 9L 15 17L 18 12L 22 20z' fill='rgb(100,100,100)' stroke='#57b5b1' stroke-width='1'></path>
        
        <path d='M 9.5 14L 11 10L 12.75 14M 17 15L 18 13L 19 15' fill='rgb(240,240,245)'></path>
        </g>
        
        <text id='mountainTopFieldBoost_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='coconutFieldBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(217, 194, 60)'></rect>
        <rect id='coconutFieldBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <circle cx='15' cy='15' r='8' fill='rgb(168, 86, 18)' stroke='#57b5b1' stroke-width='1'></circle>
        <circle cx='20' cy='14' r='1.25' fill='rgb(0,0,0,0.3)'></circle>
        <circle cx='15' cy='13' r='1.25' fill='rgb(0,0,0,0.3)'></circle>
        <circle cx='17' cy='17' r='1.25' fill='rgb(0,0,0,0.3)'></circle>
        
        <text id='coconutFieldBoost_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='pepperPatchBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(217, 194, 60)'></rect>
        <rect id='pepperPatchBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='scale(1.1,1.1) translate(0,-5)'>
        <path d='M 6 20C 11 18 12 16 13 14L 15 15C 15 22 6 22 6 20' fill='rgb(232, 42, 39)' stroke='#57b5b1' stroke-width='1'></path>
        <path d='M 14 14.5L 15.5 11.5' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='2.5'></path>
        <path d='M 14 14.5L 15 12' fill='rgb(0,0,0,0)' stroke='rgb(12, 173, 12)' stroke-width='1.5'></path>
        </g>
        <g transform='scale(1.5,1.5) translate(4,-8) rotate(14)'>
        <path d='M 6 20C 11 18 12 16 13 14L 15 15C 15 22 6 22 6 20' fill='rgb(252, 13, 13)' stroke='#57b5b1' stroke-width='1'></path>
        <path d='M 14 14.5L 15.5 11.5' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='2.5'></path>
        <path d='M 14 14.5L 15 12' fill='rgb(0,0,0,0)' stroke='rgb(12, 173, 12)' stroke-width='1.5'></path>
        </g>
        
        <text id='pepperPatchBoost_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='dandelionFieldWinds' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,230,255)'></rect>
        <rect id='dandelionFieldWinds_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <circle cx='13' cy='11' r='5' fill='rgb(240,240,240)' stroke='#57b5b1' stroke-width='2'></circle>
        <path d='M16 14C 19 19 19 22 17.5 26.5' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='5'></path>
        <path d='M16 14C 19 19 19 22 18 25' fill='rgb(0,0,0,0)' stroke='rgb(0,170,0)' stroke-width='2'></path>
        
        <text id='dandelionFieldWinds_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='sunflowerFieldWinds' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,230,255)'></rect>
        <rect id='sunflowerFieldWinds_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='translate(5,5)scale(0.7,0.7)'>
        <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(255,255,0)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15)'></path>
        <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(255,255,0)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(45)'></path>
        <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(255,255,0)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(90)'></path>
        <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(255,255,0)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(135)'></path>
        <circle cx='15' cy='15' r='5' fill='rgb(145, 101, 29)'></circle>
        </g>
        <text id='sunflowerFieldWinds_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='blueFlowerFieldWinds' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,230,255)'></rect>
        <rect id='blueFlowerFieldWinds_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='translate(5,5)scale(0.7,0.7)'>
        <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15)'></path>
        <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(60)'></path>
        <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(120)'></path>
        <circle cx='15' cy='15' r='5' fill='rgb(255,255,0)'></circle>
        </g>
        <text id='blueFlowerFieldWinds_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='mushroomFieldWinds' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,230,255)'></rect>
        <rect id='mushroomFieldWinds_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <path d='M11 17C 2 18 2 4 15 5M19 17C 28 18 29 4 15 5' fill='rgb(255,40,60)' stroke='#57b5b1' stroke-width='2'></path>
        <path d='M15 18C 2 18 2 4 15 5M15 18C 28 18 29 4 15 5' fill='rgb(255,40,60)'></path>
        <path d='M11 17C 8 30 22 30 19 17' fill='rgb(240,240,240)' stroke='#57b5b1' stroke-width='2'></path>
        <circle cx='11' cy='9' r='1.5' fill='rgb(240,240,240)'></circle>
        <circle cx='26' cy='10' r='2' fill='rgb(240,240,240)' transform='scale(0.8,1)'></circle>
        <circle cx='15' cy='19' r='1.75' fill='rgb(240,240,240)' transform='scale(1,0.7)'></circle>
        
        <text id='mushroomFieldWinds_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='cloverFieldWinds' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,230,255)'></rect>
        <rect id='cloverFieldWinds_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='translate(4,-4) rotate(10)'>
        <path d='M 18 16C 13 19 16 22 20.3 26' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='5'></path>
        <path d='M 18 16C 13 19 16 22 19 25' fill='rgb(0,0,0,0)' stroke='rgb(0,170,0)' stroke-width='2'></path>
        <g transform='rotate(24)'>
        <circle cx='0' cy='0' r='5' fill='rgb(150,220,150)' stroke='#57b5b1' stroke-width='2' transform='translate(13,8) scale(1,0.6)'></circle><circle cx='0' cy='0' r='1.5' fill='rgb(0,150,0)' transform='translate(13,8) scale(1,0.3)'></circle></g>
        <g transform='rotate(24)'>
        <circle cx='0' cy='0' r='5' fill='rgb(150,220,150)' stroke='#57b5b1' stroke-width='2' transform='translate(24,8) scale(1,0.6)'></circle><circle cx='0' cy='0' r='1.5' fill='rgb(0,150,0)' transform='translate(24,8) scale(1,0.3)'></circle></g>
        <g transform='rotate(114)'>
        <circle cx='0' cy='0' r='5' fill='rgb(150,220,150)' stroke='#57b5b1' stroke-width='2' transform='translate(13,-18) scale(1,0.6)'></circle><circle cx='0' cy='0' r='1.5' fill='rgb(0,150,0)' transform='translate(13,-18) scale(1,0.3)'></circle></g>
        <g transform='rotate(114)'>
        <circle cx='0' cy='0' r='5' fill='rgb(150,220,150)' stroke='#57b5b1' stroke-width='2' transform='translate(3,-18) scale(1,0.6)'></circle><circle cx='0' cy='0' r='1.5' fill='rgb(0,150,0)' transform='translate(3,-18) scale(1,0.3)'></circle></g>
        </g>
        
        <text id='cloverFieldWinds_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='strawberryFieldWinds' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,230,255)'></rect>
        <rect id='strawberryFieldWinds_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='scale(0.85,0.85) translate(4,5)'>
        <path d='M 13 24C 2 12 2 4 15 5M15 5C 29 5 22 18 12 24' fill='rgb(255,40,60)' stroke='#57b5b1' stroke-width='2'></path>
        
        <g transform='rotate(62) scale(0.7,1) translate(1,-14)'>
        <path d='M 6.8 4C 13 8 16 5 19 1M 15 -2C 13 8 13 5 14 10' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='4'></path>
        <path d='M 8 5C 13 8 16 5 18 2M 15 -1C 13 8 13 5 14 9' fill='rgb(0,0,0,0)' stroke='rgb(0,170,0)' stroke-width='2'></path>
        </g>
        
        <circle cx='10' cy='12' r='1' fill='rgb(255,255,40)'></circle><circle cx='18' cy='10' r='1' fill='rgb(255,255,40)'></circle><circle cx='13' cy='19' r='1' fill='rgb(255,255,40)'></circle><circle cx='15' cy='14' r='1' fill='rgb(255,255,40)'></circle>
        </g>
        
        <text id='strawberryFieldWinds_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='spiderFieldWinds' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,230,255)'></rect>
        <rect id='spiderFieldWinds_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='scale(0.9,0.9) rotate(-10) translate(-1,3)'>
        <path d='M 15 12C 8 11 10 11 7 10M 13 14C 8 16 7 14 5 14M 13 18C 8 19 7 19 4 18M 13 21C 11 23 7 22 11 24' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='2.75'></path>
        <path d='M 15 12C 8 11 10 11 8 10M 13 14C 8 16 7 14 6 14M 13 18C 8 19 7 19 5 18M 13 21C 11 23 7 22 10 24' fill='rgb(0,0,0,0)' stroke='rgb(70,70,70)' stroke-width='1'></path>
        
        <g transform='scale(-1,1) translate(-30,0)'>
        <path d='M 15 12C 8 11 10 11 7 10M 13 14C 8 16 7 14 5 14M 13 18C 8 19 7 19 4 18M 13 21C 11 23 7 22 10 24' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='2.75'></path>
        <path d='M 15 12C 8 11 10 11 8 10M 13 14C 8 16 7 14 6 14M 13 18C 8 19 7 19 5 18M 13 21C 11 23 7 22 11 24' fill='rgb(0,0,0,0)' stroke='rgb(70,70,70)' stroke-width='1'></path></g>
        
        <circle cx='18.85' cy='18' r='4.5' fill='rgb(70,70,70)' transform='scale(0.8,1)' stroke='#57b5b1' stroke-width='1.5'></circle>
        <circle cx='15' cy='12' r='3' fill='rgb(70,70,70)' stroke='#57b5b1' stroke-width='1.5'></circle>
        <circle cx='15' cy='15' r='1.9' fill='rgb(70,70,70)'></circle></g>
        
        <text id='spiderFieldWinds_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='bambooFieldWinds' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,230,255)'></rect>
        <rect id='bambooFieldWinds_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <path d='M 13 4.5L12 23.5' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='6'></path>
        <path d='M 13 7L12 22' fill='rgb(0,0,0,0)' stroke='rgb(120,200,70)' stroke-width='3'></path>
        <path d='M 11 11L 14.5 9M 11 15L 14.2 13M 10.5 20L 14 18' fill='rgb(0,0,0,0)' stroke='rgb(70,150,80)' stroke-width='1'></path>
        <circle cx='13' cy='9' r='1.6' fill='rgb(70,150,80)' transform='scale(1,0.7)'></circle>
        
        <g transform='scale(0.75,0.75) translate(12.5,8) rotate(5)'>
        <path d='M 13 4.5L12 23.5' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='6'></path>
        <path d='M 13 7L12 22' fill='rgb(0,0,0,0)' stroke='rgb(120,200,70)' stroke-width='3'></path>
        <path d='M 11 11L 14.5 9M 11 15L 14.2 13M 10.5 20L 14 18' fill='rgb(0,0,0,0)' stroke='rgb(70,150,80)' stroke-width='1'></path>
        <circle cx='13' cy='9' r='1.6' fill='rgb(70,150,80)' transform='scale(1,0.7)'></circle>
        </g>
        
        
        <text id='bambooFieldWinds_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='pineapplePatchWinds' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,230,255)'></rect>
        <rect id='pineapplePatchWinds_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='translate(-1,3) rotate(-4)'>
        <path d='M 10.5 5.5L15 9M 20.5 5.5L15 9M 14 1L15 9M 17.5 2L15 9' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='3'></path>
        <path d='M 11 6L15 9M 20 6L15 9M 14 2L15 9M 17 3L15 9' fill='rgb(0,0,0,0)' stroke='rgb(0,220,00)' stroke-width='1.5'></path>
        <circle cx='15' cy='13' r='5.5' fill='rgb(255,220,0)' transform='scale(1,1.25)' stroke='#57b5b1' stroke-width='1'></circle>
        <path d='M 10.5 13C 13 15 17 15 19.5 13M 10 18C 13 20.5 17 20.5 20 18M 13 12C 13 12 15 18 13 21M 16 12C 17 13 18 18 16 21' fill='rgb(0,0,0,0)' stroke='rgb(212, 177, 49)' stroke-width='1'></path>
        </g>
        
        <text id='pineapplePatchWinds_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='stumpFieldWinds' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,230,255)'></rect>
        <rect id='stumpFieldWinds_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='scale(1,0.9) scale(0.93,0.93) rotate(-7) translate(-1,5)'>
        <path d='M 8 9L 8 15 L 4 22L 10 18L 13 23L 18 18L 25 22L22 15L22 9C 22 4 8 4 8 9' fill='rgb(184, 116, 39)' stroke='#57b5b1' stroke-width='1'></path>
        <circle cx='15' cy='18' r='5' fill='rgb(245, 223, 101)' transform='scale(1,0.5)'></circle>
        <circle cx='15' cy='18' r='4' fill='rgb(207, 148, 54)' transform='scale(1,0.5)'></circle>
        <circle cx='15' cy='18' r='2.5' fill='rgb(245, 223, 101)' transform='scale(1,0.5)'></circle>
        <circle cx='15' cy='18' r='1' fill='rgb(207, 148, 54)' transform='scale(1,0.5)'></circle></g>
        
        <text id='stumpFieldWinds_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='cactusFieldWinds' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,230,255)'></rect>
        <rect id='cactusFieldWinds_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <path d='M 13 18L7 18L 7 11M 16 15L 22 15L 22 7' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='3'></path>
        
        <circle cx='30' cy='12' r='8' fill='rgb(0,230,50)' transform='scale(0.5,1.2)' stroke='#57b5b1' stroke-width='1'></circle>
        
        <path d='M 13 18L7 18L 7 12M 16 15L 22 15L 22 8' fill='rgb(0,0,0,0)' stroke='rgb(0,230,50)' stroke-width='2'></path>
        
        <path d='M 17 21L 19 20M 12 19L 11 21M 7 19L 7 21M 8 12L 9 10M 20 14L 21 12M 15 17L 14 15M 14 9L 12 10M 17 7L 16 5' fill='rgb(0,0,0,0)' stroke='rgb(255,255,200)' stroke-width='1'></path>
        
        <text id='cactusFieldWinds_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='pumpkinPatchWinds' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,230,255)'></rect>
        <rect id='pumpkinPatchWinds_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <path d='M 15 10C 15 7 16 6 16.75 5.25' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='3.5'></path>
        
        <circle cx='15' cy='19' r='8' fill='rgb(242, 191, 51)' transform='scale(1,0.9)' stroke='#57b5b1' stroke-width='1'></circle>
        
        <path d='M 15 10C 15 7 16 6 16 6' fill='rgb(0,0,0,0)' stroke='rgb(171, 118, 19)' stroke-width='2'></path>
        
        <path d='M 13 13C 11 13 11 21 13 21M 17 13C 19 13 19 21 17 21' fill='rgb(0,0,0,0)' stroke='rgb(0,0,0,0.2)' stroke-width='1'></path>
        
        <text id='pumpkinPatchWinds_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='pineTreeForestWinds' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,230,255)'></rect>
        <rect id='pineTreeForestWinds_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='scale(0.9,1) rotate(-5) translate(1,-1)'>
        <path d='M 15 22L 15 28' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='5'></path>
        
        <path d='M 15 22L 15 27' fill='rgb(0,0,0,0)' stroke='rgb(171, 118, 19)' stroke-width='3'></path>
        
        <path d='M 15 7L 11 12L 13 12L 8 18L 12 18L 7 23L15 23' fill='rgb(0,160,0)' stroke='#57b5b1' stroke-width='1'></path>
        <path d='M 15 7L 11 12L 13 12L 8 18L 12 18L 7 23L15 23' fill='rgb(0,160,0)' stroke='#57b5b1' stroke-width='1' transform='translate(-15,0) scale(-1,1) translate(-44.75,0)'></path></g>
        
        <text id='pineTreeForestWinds_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='roseFieldWinds' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,230,255)'></rect>
        <rect id='roseFieldWinds_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <path d='M 16 16C 17 18 17 22 15.95 25M 11 17L 17 18L 21.5 12' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='3.5'></path>
        
        <circle cx='15' cy='9' r='5' fill='rgb(237, 23, 23)' transform='scale(1,1.2)' stroke='#57b5b1' stroke-width='1'></circle>
        <circle cx='14' cy='10' r='3' fill='rgb(237, 23, 23)' transform='scale(1,0.9)' stroke='rgb(0,0,0,0.2)' stroke-width='0.5'></circle>
        <circle cx='15' cy='9' r='3' fill='rgb(237, 23, 23)' transform='scale(1,0.9)' stroke='rgb(0,0,0,0.2)' stroke-width='0.5'></circle>
        <circle cx='14.5' cy='8.5' r='2' fill='rgb(237, 23, 23)' transform='scale(1,0.9)' stroke='rgb(0,0,0,0.2)' stroke-width='0.5'></circle>
        <circle cx='15.5' cy='9.5' r='1' fill='rgb(237, 23, 23)' transform='scale(1,0.9)' stroke='rgb(0,0,0,0.2)' stroke-width='0.5'></circle>
        
        <path d='M 16 16C 17 18 17 22 16 24M 12 17L 17 18L 21 13' fill='rgb(0,0,0,0)' stroke='rgb(0,140,0)' stroke-width='2'></path>
        
        <text id='roseFieldWinds_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='mountainTopFieldWinds' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,230,255)'></rect>
        <rect id='mountainTopFieldWinds_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='rotate(-10) scale(1.2,1.2) translate(-4,-1)'>
        <path d='M 6 20L 11 9L 15 17L 18 12L 22 20z' fill='rgb(100,100,100)' stroke='#57b5b1' stroke-width='1'></path>
        
        <path d='M 9.5 14L 11 10L 12.75 14M 17 15L 18 13L 19 15' fill='rgb(240,240,245)'></path>
        </g>
        
        <text id='mountainTopFieldWinds_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='coconutFieldWinds' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,230,255)'></rect>
        <rect id='coconutFieldWinds_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <circle cx='15' cy='15' r='8' fill='rgb(168, 86, 18)' stroke='#57b5b1' stroke-width='1'></circle>
        <circle cx='20' cy='14' r='1.25' fill='rgb(0,0,0,0.3)'></circle>
        <circle cx='15' cy='13' r='1.25' fill='rgb(0,0,0,0.3)'></circle>
        <circle cx='17' cy='17' r='1.25' fill='rgb(0,0,0,0.3)'></circle>
        
        <text id='coconutFieldWinds_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='pepperPatchWinds' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,230,255)'></rect>
        <rect id='pepperPatchWinds_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform='scale(1.1,1.1) translate(0,-5)'>
        <path d='M 6 20C 11 18 12 16 13 14L 15 15C 15 22 6 22 6 20' fill='rgb(232, 42, 39)' stroke='#57b5b1' stroke-width='1'></path>
        <path d='M 14 14.5L 15.5 11.5' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='2.5'></path>
        <path d='M 14 14.5L 15 12' fill='rgb(0,0,0,0)' stroke='rgb(12, 173, 12)' stroke-width='1.5'></path>
        </g>
        <g transform='scale(1.5,1.5) translate(4,-8) rotate(14)'>
        <path d='M 6 20C 11 18 12 16 13 14L 15 15C 15 22 6 22 6 20' fill='rgb(252, 13, 13)' stroke='#57b5b1' stroke-width='1'></path>
        <path d='M 14 14.5L 15.5 11.5' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='2.5'></path>
        <path d='M 14 14.5L 15 12' fill='rgb(0,0,0,0)' stroke='rgb(12, 173, 12)' stroke-width='1.5'></path>
        </g>
        
        <text id='pepperPatchWinds_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
    </svg>

    <svg id='bearMorph' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(200,200,200)'></rect>
        <rect id='bearMorph_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>

        <text id='bearMorph_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
        <g transform=' translate(-7.5,-7.5) scale(1.5,1.5) '>
        <rect x='9' y='10' width='12' height='12' fill='rgb(66, 66, 66)'></rect>
        <path fill='rgb(120, 120, 120)' d='M9 22 C11 10 14 10 14 19C13 20 13 20 16 19C16 10 19 10 21 22'></path>
        <circle cx='10' cy='10' r='3' fill='rgb(66, 66, 66)'></circle><circle cx='20' cy='10' r='3' fill='rgb(66, 66, 66)'></circle>
        <circle cx='13' cy='15' r='0.7' fill='rgb(99, 79, 36)'></circle><circle cx='17' cy='15' r='0.7' fill='rgb(99, 79, 36)'></circle>
        </g>
        
    </svg>

    <svg id='bearMorph_' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(255,215,0,0.8)'></rect>
        <rect id='bearMorph__cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>

        <text id='bearMorph__amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
        <g transform=' translate(-7.5,-7.5) scale(1.5,1.5) '>
        <rect x='9' y='10' width='12' height='12' fill='rgb(66, 66, 66)'></rect>
        <path fill='rgb(120, 120, 120)' d='M9 22 C11 10 14 10 14 19C13 20 13 20 16 19C16 10 19 10 21 22'></path>
        <circle cx='10' cy='10' r='3' fill='rgb(66, 66, 66)'></circle><circle cx='20' cy='10' r='3' fill='rgb(66, 66, 66)'></circle>
        <circle cx='13' cy='15' r='0.7' fill='rgb(99, 79, 36)'></circle><circle cx='17' cy='15' r='0.7' fill='rgb(99, 79, 36)'></circle>
        </g>
        
    </svg>
    
    <svg id='festiveCheer' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(0,255,0,0.8)'></rect>
        <rect id='festiveCheer_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>

        <text id='festiveCheer_amount' x='28' y='28' style='font-family:calibri;font-size:11px;' text-anchor='end'></text>
        
        <g transform=' translate(4,3) scale(1,1)' fill='rgb(180,0,0)'>
        
    		<path d="M21.426,10.665l-1.062-0.25c1.281-2.281-0.222-3.743-1.687-4C16.039,5.953,14,7.71,11.993,7.682
    			c0.621-0.35,1.328-1.127,1.88-2.144c0.889-1.631,1.047-3.258,0.354-3.635c-0.606-0.331-1.666,0.41-2.516,1.695
    			c-0.055-0.164-0.104-0.327-0.173-0.492C10.616,0.91,8.81-0.425,7.503,0.124C6.199,0.672,5.889,2.896,6.812,5.093
    			c0.47,1.117,1.168,2.006,1.912,2.543C6.998,6.931,5.362,5.413,3.504,5.705C1.582,6.008,1.362,8.883,0.551,8.758l0.438,2.375
    			c1.812,0.03,1.875-3.032,3.688-3.25c2.526-0.305,4.315,1.04,5.906,0.315c0.227-0.103,0.387-0.17,0.467-0.221
    			c0.041,0,0.074,0.004,0.116,0.002c0.604,0.716,4.229-0.659,6.67-0.031c1.207,0.31,0.857,2.559,0.652,3.747L21.426,10.665z
    			 M10.621,7.537c-0.098,0.041-0.201,0.063-0.306,0.072c-0.006,0.001-0.013,0.002-0.019,0.004C9.513,7.665,8.57,6.864,8.058,5.644
    			c-0.586-1.395-0.389-2.808,0.44-3.156c0.829-0.348,1.977,0.499,2.563,1.895c0.034,0.081,0.056,0.16,0.084,0.241
    			c-0.544,1.173-0.697,2.272-0.418,2.847C10.69,7.491,10.661,7.52,10.621,7.537z M11.044,7.185c-0.175-0.375-0.068-1.092,0.294-1.85
    			C11.47,6.092,11.361,6.768,11.044,7.185z M11.587,7.434c0.473-0.706,0.597-1.824,0.341-3.046c0.484-0.596,1.017-0.908,1.343-0.729
    			c0.439,0.239,0.34,1.273-0.225,2.309C12.613,6.764,12.033,7.325,11.587,7.434z"/>
    		<rect x="6.519" y="13.446" width="2.438" height="8.531"/>
    		<rect x="4.957" y="13.446" width="1" height="8.531"/>
    		<rect x="9.519" y="13.446" width="2.875" height="8.531"/>
    		<rect x="15.957" y="13.446" width="1" height="8.531"/>
    		<rect x="12.957" y="13.446" width="2.438" height="8.531"/>
    		<rect x="4.457" y="8.977" width="13" height="4"/>
        </g>
        
    </svg>
    
    <svg id='gummyMorph' style='width:30px;height:30px;display:none;'>
        <rect width='30' height='30' fill='rgb(255,50,255,0.75)'></rect>
        
        <path fill='rgb(25,255,126,0.75)' d='M9 21 C11 10 14 10 14 19C13 20 13 20 16 19C16 10 19 10 21 21'></path>
        <circle cx='10' cy='10' r='3' fill='rgb(25,255,126,0.75)'></circle><circle cx='20' cy='10' r='3' fill='rgb(25,255,126,0.75)'></circle>
        <circle cx='13' cy='15' r='0.7' fill='rgb(0,0,0)'></circle><circle cx='17' cy='15' r='0.7' fill='rgb(0,0,0)'></circle>
        
        <rect id='gummyMorph_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='gummyMorph_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>

    <svg id='conversionBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(255,215,100)'></rect>
        <rect id='conversionBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <path fill='rgb(255, 214, 0)' stroke='rgb(0,0,0)' stroke-width='0.8' d='M13 10C16 10 24 17 15 20C8 12 18 15 13 10Z' transform='scale(1.8,1.8) translate(-7,-6)'></path>
        
        <text id='conversionBoost_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='haste' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(255,255,255)'></rect>
        <rect id='haste_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <path d='M18 9L15 21M12 26L15 21M16 26L15 21M11 11L16 15M23 15L16 15' stroke='rgb(130,130,130)' stroke-width='2'></path>
        <path d='M5 7 L11 7M2 15L12 15M3 23L11 23' stroke='rgb(130,130,130)' stroke-width='1.5' opacity='0.5'></path>
        <circle cx='18' cy='9' r='4'fill='rgb(130,130,130)'></circle>
        <text id='haste_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>

    <svg id='haste_' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(255,220,10)'></rect>
        <rect id='haste__cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <path d='M18 9L15 21M12 26L15 21M16 26L15 21M11 11L16 15M23 15L16 15' stroke='rgb(130,130,130)' stroke-width='2'></path>
        <path d='M5 7 L11 7M2 15L12 15M3 23L11 23' stroke='rgb(130,130,130)' stroke-width='1.5' opacity='0.5'></path>
        <circle cx='18' cy='9' r='4'fill='rgb(130,130,130)'></circle>
        <text id='haste__amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>

    <svg id='coconutSurge' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(138, 80, 33)'></rect>
        <rect id='coconutSurge_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <path d='M18 9L15 21M12 26L15 21M16 26L15 21M11 11L16 15M23 15L16 15' stroke='rgb(130,130,130)' stroke-width='2'></path>
        <path d='M5 7 L11 7M2 15L12 15M3 23L11 23' stroke='rgb(130,130,130)' stroke-width='1.5' opacity='0.5'></path>
        <circle cx='18' cy='9' r='4'fill='rgb(130,130,130)'></circle>
        <text id='coconutSurge_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='focus' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(0,200,0)'></rect>
        <rect id='focus_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <path fill='rgb(0,0,0)' d='M5 15C10 0 20 0 25 15M5 15C10 30 20 30 25 15'></path>
        <circle cx='15' cy='15' r='3' fill='rgb(0,200,0)'></circle>
        <text id='focus_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='melody' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(255,255,255)'></rect>
        <rect id='melody_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <path d='M6 22L8 8M8 8L23 8M23 8L20 22' stroke='rgb(0,0,0)' stroke-width='3'></path>
        <circle cx='5' cy='22' r='3'fill='rgb(0,0,0)'></circle>
        <circle cx='19' cy='22' r='3'fill='rgb(0,0,0)'></circle>
        <text id='melody_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='bombCombo' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(0,0,0)'></rect>
        <path d='M15 26 L17.169 19.505 L23.600 21.858 L19.875 16.113 L25.724 12.552 L18.909 11.883 L19.773 5.089 L15 10 L10.227 5.089 L11.091 11.883 L4.276 12.552 L10.125 16.113 L6.400 21.858 L12.831 19.505' fill='rgb(255,255,255)'></path>
        <rect id='bombCombo_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='bombCombo_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' fill='rgb(255,255,255)' text-anchor='end'></text>
        
    </svg>
    
    <svg id='blueBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(150,255,150)'></rect>
        <rect id='blueBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <path fill='rgb(78, 135, 242)' stroke='rgb(0,0,0)' stroke-width='1' d='M-3 -3C-9 -15 9 -15 3 -3' transform='translate(15,15)'></path>
        <path fill='rgb(78, 135, 242)' stroke='rgb(0,0,0)' stroke-width='1' d='M-3 -3C-9 -15 9 -15 3 -3' transform='translate(15,15) rotate(90)'></path>
        <path fill='rgb(78, 135, 242)' stroke='rgb(0,0,0)' stroke-width='1' d='M-3 -3C-9 -15 9 -15 3 -3' transform='translate(15,15) rotate(180)'></path>
        <path fill='rgb(78, 135, 242)' stroke='rgb(0,0,0)' stroke-width='1' d='M-3 -3C-9 -15 9 -15 3 -3' transform='translate(15,15) rotate(270)'></path>
        <rect x='12' y='12' width='6' height='6' fill='rgb(78, 135, 242)'></rect>
        <path stroke='rgb(255,255,255)' stroke-width='2' d='M10 15L20 15M15 10L15 20'></path>
        <text id='blueBoost_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='redBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(150,255,150)'></rect>
        <rect id='redBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <path fill='rgb(237, 82, 73)' stroke='rgb(0,0,0)' stroke-width='1' d='M-3 -3C-9 -15 9 -15 3 -3' transform='translate(15,15)'></path>
        <path fill='rgb(237, 82, 73)' stroke='rgb(0,0,0)' stroke-width='1' d='M-3 -3C-9 -15 9 -15 3 -3' transform='translate(15,15) rotate(90)'></path>
        <path fill='rgb(237, 82, 73)' stroke='rgb(0,0,0)' stroke-width='1' d='M-3 -3C-9 -15 9 -15 3 -3' transform='translate(15,15) rotate(180)'></path>
        <path fill='rgb(237, 82, 73)' stroke='rgb(0,0,0)' stroke-width='1' d='M-3 -3C-9 -15 9 -15 3 -3' transform='translate(15,15) rotate(270)'></path>
        <rect x='12' y='12' width='6' height='6' fill='rgb(237, 82, 73)'></rect>
        <path stroke='rgb(255,255,255)' stroke-width='2' d='M10 15L20 15M15 10L15 20'></path>
        <text id='redBoost_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='whiteBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(150,255,150)'></rect>
        <rect id='whiteBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <path fill='rgb(200,200,200)' stroke='rgb(0,0,0)' stroke-width='1' d='M-3 -3C-9 -15 9 -15 3 -3' transform='translate(15,15)'></path>
        <path fill='rgb(200,200,200)' stroke='rgb(0,0,0)' stroke-width='1' d='M-3 -3C-9 -15 9 -15 3 -3' transform='translate(15,15) rotate(90)'></path>
        <path fill='rgb(200,200,200)' stroke='rgb(0,0,0)' stroke-width='1' d='M-3 -3C-9 -15 9 -15 3 -3' transform='translate(15,15) rotate(180)'></path>
        <path fill='rgb(200,200,200)' stroke='rgb(0,0,0)' stroke-width='1' d='M-3 -3C-9 -15 9 -15 3 -3' transform='translate(15,15) rotate(270)'></path>
        <rect x='12' y='12' width='6' height='6' fill='rgb(200,200,200)'></rect>
        <path stroke='rgb(255,255,255)' stroke-width='2' d='M10 15L20 15M15 10L15 20'></path>
        <text id='whiteBoost_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='babyLove' style='width:30px;height:30px;display:none'>
    
        <rect width='30' height='30' fill='rgb(140, 227, 244)'></rect>
        <rect id='babyLove_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <path stroke='rgb(50, 50, 50)' stroke-width='1.5' d='M5 10C 7 5 13 5 15 9C 19 5 24 6 24 11C 25 14 14 20 15 25C 10 14 6 19 5 10' fill='rgb(240, 177, 226)' transform='scale(1.2,1.2) translate(-2,-3)'></path>
        <circle cx='12.4' cy='11.4' r='1' fill='rgb(70,70,70)'></circle>
        <circle cx='18.4' cy='10.5' r='1' fill='rgb(70,70,70)'></circle>
        <circle cx='15.8' cy='12.2' r='0.75' fill='rgb(70,70,70)'></circle>
        <circle cx='10.2' cy='23.4' r='1.5' fill='rgb(234, 34, 105)' transform='scale(1,0.6)'></circle>
        <circle cx='20.5' cy='21.6' r='1.5' fill='rgb(234, 34, 105)' transform='scale(1,0.6)'></circle>
        <path stroke='rgb(70, 70, 70)' stroke-width='0.8' d='M 13 15.5C 14 15 16 17.6 18 14.5C 17 19 15 18.2 13 15.5' fill='rgb(220,220,220)'></path>
        
        <text id='babyLove_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='inspire' style='width:30px;height:30px;display:none'>
    
        <rect width='30' height='30' fill='rgb(100, 100, 100)'></rect>
        <rect id='inspire_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <path stroke='rgb(255, 241, 43)' stroke-width='1.5' d='M23.78 7.73L29.39 40.45L0.00 25.00L-29.39 40.45L-23.78 7.73L-47.55 -15.45L-14.69 -20.23L-0.00 -50.00L14.69 -20.23L47.55 -15.45L23.78 7.73' fill='rgb(255, 241, 43)' transform='scale(0.2,0.2) translate(75,75)'></path>
        <text id='inspire_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='rage' style='width:30px;height:30px;display:none'>
    
        <rect width='30' height='30' fill='rgb(255, 149, 125)'></rect>
        <rect id='rage_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <path stroke='rgb(255, 20, 0)' stroke-width='2' d='M5 10C5 5 10 5 14 11M25 10C25 5 20 5 16 11M7 25 C 7 15 23 15 23 25M10 10L 10 15M20 10 L 20 15' fill='rgb(0,0,0,0)'></path>
        <text id='rage_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>

    <svg id='flameHeat' style='width:30px;height:30px;display:none;'>
        
        <rect width='30' height='30' fill='rgb(255,20,20)'></rect>
        <rect id='flameHeat_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <path d='M15 25C5 24 5 13 15 5M15 25C25 24 25 13 15 5' fill='rgb(255,150,20)'></path>
        <path d='M15 25C10 24 10 17 15 10M15 25C20 24 20 17 15 10' fill='rgb(205,100,0)'></path>
        <text id='flameHeat_amount' x='29' y='29' style='font-family:calibri;font-size:12px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='darkHeat' style='width:30px;height:30px;display:none;'>
        
        <rect width='30' height='30' fill='rgb(205,0,205)'></rect>
        <rect id='darkHeat_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <path d='M15 25C5 24 5 13 15 5M15 25C25 24 25 13 15 5' fill='rgb(255,0,0)'></path>
        <path d='M15 25C10 24 10 17 15 10M15 25C20 24 20 17 15 10' fill='rgb(205,0,0)'></path>
        <text id='darkHeat_amount' x='29' y='29' style='font-family:calibri;font-size:12px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='pollenMark' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(255,255,255)'></rect>
        <rect id='pollenMark_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <circle cx='15' cy='15' r='12'fill='rgb(255,255,180)'></circle>
        <path d='M7 7L22 22M22 7L7 22' stroke='rgb(0,0,0)' stroke-width='1.2'></path>
        <circle cx='15' cy='15' r='7'fill='rgb(180,180,180)'></circle>
        <text id='pollenMark_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='honeyMark' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(255,255,255)'></rect>
        <rect id='honeyMark_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <circle cx='15' cy='15' r='12'fill='rgb(180,180,180)'></circle>
        <path d='M7 7L22 22M22 7L7 22' stroke='rgb(0,0,0)' stroke-width='1.2'></path>
        <circle cx='15' cy='15' r='7'fill='rgb(255,226,8)'></circle>
        <text id='honeyMark_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='preciseMark' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(255,255,255)'></rect>
        <rect id='preciseMark_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <circle cx='15' cy='15' r='12'fill='rgb(180,180,180)'></circle>
        <path d='M7 7L22 22M22 7L7 22' stroke='rgb(0,0,0)' stroke-width='1.2'></path>
        <circle cx='15' cy='15' r='7'fill='rgb(143,0,236)'></circle>
        <text id='preciseMark_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='balloonAura' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(255,255,0)'></rect>
        <rect id='balloonAura_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <circle cx='12' cy='12' r='7'fill='rgb(0,0,255)'></circle>
        <path stroke='rgb(200,200,200)' stroke-width='2' d='M13 18C10 20 12 25 20 26' fill='rgba(0,0,0,0)'></path>
        <text id='balloonAura_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='cloudBoost' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(150,150,200)'></rect>
        <rect id='cloudBoost_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <circle cx='15' cy='14' r='5'fill='rgb(255,255,255)'></circle>
        <circle cx='10' cy='16' r='3'fill='rgb(255,255,255)'></circle>
        <circle cx='21' cy='15' r='4'fill='rgb(255,255,255)'></circle>
        <path stroke='rgb(255,255,255)' stroke-width='1.5' d='M 12 22L 11 27M 18 20L 17 25M 22 22L 21 27' fill='rgb(0,0,0,0)'></path>
        <text id='cloudBoost_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='flameFuel' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(230,0,0)'></rect>
        <rect id='flameFuel_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <path d='M15 25C5 24 5 13 15 5M15 25C25 24 25 13 15 5' fill='rgb(30,0,0)'></path>
        <path d='M15 25C10 24 10 17 15 10M15 25C20 24 20 17 15 10' fill='rgb(105,0,0)'></path>
        <text id='flameFuel_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='balloonBlessing' style='width:30px;height:30px;display:none'>
        <rect width='30' height='30' fill='rgb(255,255,0)'></rect>
        <rect id='balloonBlessing_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <circle cx='12' cy='12' r='7'fill='rgb(0,0,255)'></circle>
        <path stroke='rgb(200,200,200)' stroke-width='2' d='M13 18C10 20 12 25 20 26' fill='rgba(0,0,0,0)'></path>
        <text id='balloonBlessing_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='precision' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(143,0,236)'></rect>
        <rect id='precision_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <path stroke='rgb(0,0,0)' stroke-width='2' d='M15 9L15 22M7 12C7 8 22 8 22 12' fill='rgb(0,0,0,0)'></path>
        <text id='precision_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='rage' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(255, 149, 125)'></rect>
        <rect id='rage_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <path stroke='rgb(255, 20, 0)' stroke-width='2' d='M5 10C5 5 10 5 14 11M25 10C25 5 20 5 16 11M7 25 C 7 15 23 15 23 25M10 10L 10 15M20 10 L 20 15' fill='rgb(0,0,0,0)'></path>
        <text id='rage_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='coconutShield' style='width:30px;height:30px;display:none;'>
        <rect width='30' height='30' fill='rgb(205,205,205)'></rect>
        
        <circle cx='15' cy='15' r='10' fill='rgb(255,255,255)'></circle>
        <circle cx='15' cy='15' r='8' fill='rgb(150,75,0)' stroke='rgb(0,0,0)' stroke-width='1'></circle>
        
        <circle cx='17' cy='16' r='1.5' fill='rgb(94, 51, 7)'></circle><circle cx='12' cy='13' r='1.5' fill='rgb(94, 51, 7)'></circle><circle cx='17' cy='12' r='1.5' fill='rgb(94, 51, 7)'></circle>
        
        <rect id='coconutShield_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='coconutShield_amount' x='29' y='29' style='font-family:tahoma;font-size:12px;' text-anchor='end' fill='rgb(255,255,255)'></text>
        
    </svg>
    
    <svg id='tidePower' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(70,130,255)'></rect>
        <rect id='tidePower_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform="translate(2,32) scale(0.009,-0.011)" fill="rgb(200,200,200)" stroke="rgb(200,200,200)" stroke-width='100'>
        <path d="M1310 2473 c-52 -6 -226 -61 -295 -93 -100 -45 -201 -117 -298 -214
        -157 -156 -281 -363 -357 -596 -58 -176 -73 -270 -73 -450 -1 -200 24 -324 86
        -440 l42 -78 68 -17 c83 -20 139 -11 223 36 54 31 58 31 140 25 46 -4 100 -14
        119 -22 63 -26 159 -18 255 21 72 30 95 35 165 35 67 0 85 3 99 19 42 46 38
        135 -8 164 -13 9 -47 17 -76 19 l-52 3 -35 85 c-59 143 -61 333 -5 492 43 121
        119 193 218 204 43 5 52 3 70 -17 34 -39 53 -101 80 -265 26 -163 44 -210 117
        -312 54 -75 74 -82 233 -82 134 0 141 1 204 31 93 44 142 88 179 161 l31 63 0
        192 c0 205 -9 274 -55 405 -66 189 -214 362 -404 472 -150 87 -376 155 -531
        161 -58 2 -121 1 -140 -2z m270 -50 c92 -17 263 -74 333 -109 197 -100 366
        -282 436 -469 49 -130 61 -209 61 -396 0 -186 -5 -216 -49 -283 l-26 -40 -8
        135 c-24 426 -47 580 -101 673 -62 106 -234 249 -391 326 -150 72 -246 93
        -397 87 -142 -6 -213 -18 -318 -53 -114 -38 -213 -89 -279 -144 -69 -57 -96
        -98 -111 -165 -12 -58 -83 -166 -194 -300 -78 -93 -121 -163 -169 -275 -20
        -49 -26 -56 -22 -30 37 219 153 475 298 654 191 238 399 359 697 405 63 10
        132 6 240 -16z m14 -118 c168 -29 319 -107 476 -245 167 -147 187 -211 215
        -675 l15 -240 -23 -18 c-12 -11 -38 -32 -56 -48 -45 -38 -66 -43 -154 -35 -39
        4 -90 8 -112 9 -53 3 -64 11 -108 84 -36 59 -38 69 -62 250 -37 279 -77 410
        -156 504 -40 48 -79 68 -64 32 17 -38 17 -83 1 -108 -9 -14 -48 -41 -87 -60
        -94 -46 -144 -84 -192 -147 -109 -144 -161 -363 -126 -530 13 -59 61 -172 78
        -182 15 -10 14 -36 -1 -36 -7 0 -35 -12 -63 -26 -75 -37 -132 -46 -196 -29
        -30 8 -93 15 -140 15 -91 0 -86 2 -234 -78 -30 -17 -56 -22 -116 -22 l-76 0
        -16 62 c-30 114 -40 192 -40 313 0 234 50 379 193 555 155 191 200 259 215
        330 4 19 13 47 21 61 37 73 171 160 334 217 73 25 122 35 260 56 58 8 133 5
        214 -9z m79 -555 c14 -36 33 -96 42 -135 19 -80 55 -324 55 -371 0 -18 14 -58
        30 -89 45 -86 28 -78 -23 10 -40 70 -46 89 -66 213 -26 166 -55 254 -96 294
        -28 28 -34 30 -92 25 -40 -3 -75 -13 -98 -27 -20 -12 -38 -20 -41 -17 -3 3 34
        27 82 54 55 30 100 64 122 91 42 51 47 48 85 -48z m-326 -142 c-75 -100 -112
        -232 -111 -398 0 -92 4 -129 23 -185 41 -124 76 -175 121 -175 50 0 88 -20 95
        -49 7 -32 -9 -80 -27 -83 -7 0 -44 -2 -83 -3 -53 -1 -89 -9 -147 -33 -88 -37
        -177 -42 -275 -17 -31 8 -87 15 -124 15 -56 0 -79 -6 -143 -36 -67 -32 -84
        -36 -134 -32 -66 5 -112 25 -122 53 -7 19 -3 20 72 22 82 3 112 13 208 68 44
        26 47 26 175 20 222 -11 235 -10 314 30 89 44 94 60 52 147 -61 128 -75 241
        -46 368 28 121 59 186 132 278 40 49 55 57 20 10z"/>
        <path d="M1130 2124 c-71 -13 -220 -90 -220 -114 0 -5 22 -10 50 -10 55 0 291
        44 353 66 52 18 54 18 73 -11 15 -23 15 -27 0 -49 -9 -14 -55 -45 -103 -70
        -284 -147 -424 -401 -449 -816 l-7 -125 -14 45 c-8 25 -17 90 -20 145 -12 215
        69 440 209 587 30 30 119 99 199 151 211 139 251 191 56 73 -156 -95 -196
        -123 -260 -185 -223 -214 -299 -585 -183 -892 44 -119 53 -95 60 176 7 242 32
        353 111 503 78 146 165 232 315 307 154 77 182 147 80 200 -26 14 -60 19 -130
        21 -52 1 -106 0 -120 -2z m125 -33 c-13 -11 -194 -45 -202 -38 -8 9 44 28 102
        37 72 11 111 11 100 1z"/>
        <path d="M1966 1749 c-32 -25 -34 -59 -5 -131 32 -80 49 -148 49 -200 0 -81
        37 -126 87 -107 31 11 43 40 43 104 0 125 -91 355 -140 355 -4 0 -19 -9 -34
        -21z m73 -81 c59 -126 88 -266 62 -306 -5 -8 -17 -12 -27 -10 -14 2 -20 19
        -30 85 -6 45 -24 117 -39 160 -27 76 -29 133 -5 133 5 0 22 -28 39 -62z"/>
        </g>
        
        <text id='tidePower_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='tidalSurge' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(18, 60, 230)'></rect>
        <rect id='tidalSurge_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform="translate(2,32) scale(0.009,-0.011)" fill="rgb(200,200,200)" stroke="rgb(200,200,200)" stroke-width='100'>
        <path d="M1310 2473 c-52 -6 -226 -61 -295 -93 -100 -45 -201 -117 -298 -214
        -157 -156 -281 -363 -357 -596 -58 -176 -73 -270 -73 -450 -1 -200 24 -324 86
        -440 l42 -78 68 -17 c83 -20 139 -11 223 36 54 31 58 31 140 25 46 -4 100 -14
        119 -22 63 -26 159 -18 255 21 72 30 95 35 165 35 67 0 85 3 99 19 42 46 38
        135 -8 164 -13 9 -47 17 -76 19 l-52 3 -35 85 c-59 143 -61 333 -5 492 43 121
        119 193 218 204 43 5 52 3 70 -17 34 -39 53 -101 80 -265 26 -163 44 -210 117
        -312 54 -75 74 -82 233 -82 134 0 141 1 204 31 93 44 142 88 179 161 l31 63 0
        192 c0 205 -9 274 -55 405 -66 189 -214 362 -404 472 -150 87 -376 155 -531
        161 -58 2 -121 1 -140 -2z m270 -50 c92 -17 263 -74 333 -109 197 -100 366
        -282 436 -469 49 -130 61 -209 61 -396 0 -186 -5 -216 -49 -283 l-26 -40 -8
        135 c-24 426 -47 580 -101 673 -62 106 -234 249 -391 326 -150 72 -246 93
        -397 87 -142 -6 -213 -18 -318 -53 -114 -38 -213 -89 -279 -144 -69 -57 -96
        -98 -111 -165 -12 -58 -83 -166 -194 -300 -78 -93 -121 -163 -169 -275 -20
        -49 -26 -56 -22 -30 37 219 153 475 298 654 191 238 399 359 697 405 63 10
        132 6 240 -16z m14 -118 c168 -29 319 -107 476 -245 167 -147 187 -211 215
        -675 l15 -240 -23 -18 c-12 -11 -38 -32 -56 -48 -45 -38 -66 -43 -154 -35 -39
        4 -90 8 -112 9 -53 3 -64 11 -108 84 -36 59 -38 69 -62 250 -37 279 -77 410
        -156 504 -40 48 -79 68 -64 32 17 -38 17 -83 1 -108 -9 -14 -48 -41 -87 -60
        -94 -46 -144 -84 -192 -147 -109 -144 -161 -363 -126 -530 13 -59 61 -172 78
        -182 15 -10 14 -36 -1 -36 -7 0 -35 -12 -63 -26 -75 -37 -132 -46 -196 -29
        -30 8 -93 15 -140 15 -91 0 -86 2 -234 -78 -30 -17 -56 -22 -116 -22 l-76 0
        -16 62 c-30 114 -40 192 -40 313 0 234 50 379 193 555 155 191 200 259 215
        330 4 19 13 47 21 61 37 73 171 160 334 217 73 25 122 35 260 56 58 8 133 5
        214 -9z m79 -555 c14 -36 33 -96 42 -135 19 -80 55 -324 55 -371 0 -18 14 -58
        30 -89 45 -86 28 -78 -23 10 -40 70 -46 89 -66 213 -26 166 -55 254 -96 294
        -28 28 -34 30 -92 25 -40 -3 -75 -13 -98 -27 -20 -12 -38 -20 -41 -17 -3 3 34
        27 82 54 55 30 100 64 122 91 42 51 47 48 85 -48z m-326 -142 c-75 -100 -112
        -232 -111 -398 0 -92 4 -129 23 -185 41 -124 76 -175 121 -175 50 0 88 -20 95
        -49 7 -32 -9 -80 -27 -83 -7 0 -44 -2 -83 -3 -53 -1 -89 -9 -147 -33 -88 -37
        -177 -42 -275 -17 -31 8 -87 15 -124 15 -56 0 -79 -6 -143 -36 -67 -32 -84
        -36 -134 -32 -66 5 -112 25 -122 53 -7 19 -3 20 72 22 82 3 112 13 208 68 44
        26 47 26 175 20 222 -11 235 -10 314 30 89 44 94 60 52 147 -61 128 -75 241
        -46 368 28 121 59 186 132 278 40 49 55 57 20 10z"/>
        <path d="M1130 2124 c-71 -13 -220 -90 -220 -114 0 -5 22 -10 50 -10 55 0 291
        44 353 66 52 18 54 18 73 -11 15 -23 15 -27 0 -49 -9 -14 -55 -45 -103 -70
        -284 -147 -424 -401 -449 -816 l-7 -125 -14 45 c-8 25 -17 90 -20 145 -12 215
        69 440 209 587 30 30 119 99 199 151 211 139 251 191 56 73 -156 -95 -196
        -123 -260 -185 -223 -214 -299 -585 -183 -892 44 -119 53 -95 60 176 7 242 32
        353 111 503 78 146 165 232 315 307 154 77 182 147 80 200 -26 14 -60 19 -130
        21 -52 1 -106 0 -120 -2z m125 -33 c-13 -11 -194 -45 -202 -38 -8 9 44 28 102
        37 72 11 111 11 100 1z"/>
        <path d="M1966 1749 c-32 -25 -34 -59 -5 -131 32 -80 49 -148 49 -200 0 -81
        37 -126 87 -107 31 11 43 40 43 104 0 125 -91 355 -140 355 -4 0 -19 -9 -34
        -21z m73 -81 c59 -126 88 -266 62 -306 -5 -8 -17 -12 -27 -10 -14 2 -20 19
        -30 85 -6 45 -24 117 -39 160 -27 76 -29 133 -5 133 5 0 22 -28 39 -62z"/>
        </g>
        <text id='tidalSurge_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='tideBlessing' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(38, 90, 220)'></rect>
        <rect id='tideBlessing_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        
        <g transform="translate(2,32) scale(0.009,-0.011)" fill="rgb(200,200,200)" stroke="rgb(200,200,200)" stroke-width='100'>
        <path d="M1310 2473 c-52 -6 -226 -61 -295 -93 -100 -45 -201 -117 -298 -214
        -157 -156 -281 -363 -357 -596 -58 -176 -73 -270 -73 -450 -1 -200 24 -324 86
        -440 l42 -78 68 -17 c83 -20 139 -11 223 36 54 31 58 31 140 25 46 -4 100 -14
        119 -22 63 -26 159 -18 255 21 72 30 95 35 165 35 67 0 85 3 99 19 42 46 38
        135 -8 164 -13 9 -47 17 -76 19 l-52 3 -35 85 c-59 143 -61 333 -5 492 43 121
        119 193 218 204 43 5 52 3 70 -17 34 -39 53 -101 80 -265 26 -163 44 -210 117
        -312 54 -75 74 -82 233 -82 134 0 141 1 204 31 93 44 142 88 179 161 l31 63 0
        192 c0 205 -9 274 -55 405 -66 189 -214 362 -404 472 -150 87 -376 155 -531
        161 -58 2 -121 1 -140 -2z m270 -50 c92 -17 263 -74 333 -109 197 -100 366
        -282 436 -469 49 -130 61 -209 61 -396 0 -186 -5 -216 -49 -283 l-26 -40 -8
        135 c-24 426 -47 580 -101 673 -62 106 -234 249 -391 326 -150 72 -246 93
        -397 87 -142 -6 -213 -18 -318 -53 -114 -38 -213 -89 -279 -144 -69 -57 -96
        -98 -111 -165 -12 -58 -83 -166 -194 -300 -78 -93 -121 -163 -169 -275 -20
        -49 -26 -56 -22 -30 37 219 153 475 298 654 191 238 399 359 697 405 63 10
        132 6 240 -16z m14 -118 c168 -29 319 -107 476 -245 167 -147 187 -211 215
        -675 l15 -240 -23 -18 c-12 -11 -38 -32 -56 -48 -45 -38 -66 -43 -154 -35 -39
        4 -90 8 -112 9 -53 3 -64 11 -108 84 -36 59 -38 69 -62 250 -37 279 -77 410
        -156 504 -40 48 -79 68 -64 32 17 -38 17 -83 1 -108 -9 -14 -48 -41 -87 -60
        -94 -46 -144 -84 -192 -147 -109 -144 -161 -363 -126 -530 13 -59 61 -172 78
        -182 15 -10 14 -36 -1 -36 -7 0 -35 -12 -63 -26 -75 -37 -132 -46 -196 -29
        -30 8 -93 15 -140 15 -91 0 -86 2 -234 -78 -30 -17 -56 -22 -116 -22 l-76 0
        -16 62 c-30 114 -40 192 -40 313 0 234 50 379 193 555 155 191 200 259 215
        330 4 19 13 47 21 61 37 73 171 160 334 217 73 25 122 35 260 56 58 8 133 5
        214 -9z m79 -555 c14 -36 33 -96 42 -135 19 -80 55 -324 55 -371 0 -18 14 -58
        30 -89 45 -86 28 -78 -23 10 -40 70 -46 89 -66 213 -26 166 -55 254 -96 294
        -28 28 -34 30 -92 25 -40 -3 -75 -13 -98 -27 -20 -12 -38 -20 -41 -17 -3 3 34
        27 82 54 55 30 100 64 122 91 42 51 47 48 85 -48z m-326 -142 c-75 -100 -112
        -232 -111 -398 0 -92 4 -129 23 -185 41 -124 76 -175 121 -175 50 0 88 -20 95
        -49 7 -32 -9 -80 -27 -83 -7 0 -44 -2 -83 -3 -53 -1 -89 -9 -147 -33 -88 -37
        -177 -42 -275 -17 -31 8 -87 15 -124 15 -56 0 -79 -6 -143 -36 -67 -32 -84
        -36 -134 -32 -66 5 -112 25 -122 53 -7 19 -3 20 72 22 82 3 112 13 208 68 44
        26 47 26 175 20 222 -11 235 -10 314 30 89 44 94 60 52 147 -61 128 -75 241
        -46 368 28 121 59 186 132 278 40 49 55 57 20 10z"/>
        <path d="M1130 2124 c-71 -13 -220 -90 -220 -114 0 -5 22 -10 50 -10 55 0 291
        44 353 66 52 18 54 18 73 -11 15 -23 15 -27 0 -49 -9 -14 -55 -45 -103 -70
        -284 -147 -424 -401 -449 -816 l-7 -125 -14 45 c-8 25 -17 90 -20 145 -12 215
        69 440 209 587 30 30 119 99 199 151 211 139 251 191 56 73 -156 -95 -196
        -123 -260 -185 -223 -214 -299 -585 -183 -892 44 -119 53 -95 60 176 7 242 32
        353 111 503 78 146 165 232 315 307 154 77 182 147 80 200 -26 14 -60 19 -130
        21 -52 1 -106 0 -120 -2z m125 -33 c-13 -11 -194 -45 -202 -38 -8 9 44 28 102
        37 72 11 111 11 100 1z"/>
        <path d="M1966 1749 c-32 -25 -34 -59 -5 -131 32 -80 49 -148 49 -200 0 -81
        37 -126 87 -107 31 11 43 40 43 104 0 125 -91 355 -140 355 -4 0 -19 -9 -34
        -21z m73 -81 c59 -126 88 -266 62 -306 -5 -8 -17 -12 -27 -10 -14 2 -20 19
        -30 85 -6 45 -24 117 -39 160 -27 76 -29 133 -5 133 5 0 22 -28 39 -62z"/>
        </g>
        <text id='tideBlessing_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='popStarAura' style='width:30px;height:30px;display:none;'>
        <rect width='30' height='30' fill='rgb(150,150,150)'></rect>
        <rect id='popStarAura_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <circle cx='6' cy='6' r='3' fill='rgb(30,110,205)' stroke='rgb(0,0,0,0.4)'></circle><circle cx='22' cy='10' r='3' fill='rgb(30,110,205)' stroke='rgb(0,0,0,0.4)'></circle><circle cx='15' cy='22' r='3' fill='rgb(30,110,205)' stroke='rgb(0,0,0,0.4)'></circle>
        <path d='M0.00000 3.00000L4.70228 6.47214L2.85317 0.92705L7.60845 -2.47214L1.76336 -2.42705L0.00000 -8.00000L-1.76336 -2.42705L-7.60845 -2.47214L-2.85317 0.92705L-4.70228 6.47214Z' transform='translate(15,15) scale(1.3,1.3)' fill='rgb(60,170,255)' stroke='rgb(10,80,200)'></path>
        <text id='popStarAura_amount' x='29' y='29' style='font-family:calibri;font-size:12px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='scorchingStarAura' style='width:30px;height:30px;display:none;'>
        <rect width='30' height='30' fill='rgb(150,150,150)'></rect>
        <rect id='scorchingStarAura_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <path d='M8 12 C5 10 5 8 10 3C15 15 15 15 13 15M19 12C15 15 25 1 25 12Z' fill='rgb(255,100,0)'></path>
        <path d='M0.00000 3.00000L4.70228 6.47214L2.85317 0.92705L7.60845 -2.47214L1.76336 -2.42705L0.00000 -8.00000L-1.76336 -2.42705L-7.60845 -2.47214L-2.85317 0.92705L-4.70228 6.47214Z' transform='translate(15,15) scale(1.3,1.3)' fill='rgb(220,0,0)' stroke='rgb(150,0,0)'></path>
        <text id='scorchingStarAura_amount' x='29' y='29' style='font-family:calibri;font-size:12px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='gummyStarAura' style='width:30px;height:30px;display:none;'>
    
        <defs>
            <linearGradient id='gummyStarGrad' x1='0.4' x2='0.6' y1='0.4' y2='0.6'>
                <stop offset='10%' stop-color='rgb(255, 50, 255)'/>
                <stop offset='90%' stop-color='rgb(25, 255, 126)'/>
            </linearGradient>
            <linearGradient id='gummyStarOutline' x1='0.4' x2='0.6' y1='0.4' y2='0.6'>
                <stop offset='10%' stop-color='rgb(0, 205, 86)'/>
                <stop offset='90%' stop-color='rgb(205, 10, 205)'/>
            </linearGradient>
        </defs>
        <rect width='30' height='30' fill='rgb(150,150,150)'></rect>
        <rect id='gummyStarAura_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <circle cx='9' cy='7' fill='rgb(205,10,205)' r='2'></circle>
        <circle cx='20' cy='7' fill='rgb(0,205,86)' r='2'></circle>
        <circle cx='24' cy='19' fill='rgb(205,10,205)' r='2'></circle>
        <circle cx='6' cy='19' fill='rgb(0,205,86)' r='2'></circle>
        <path d='M0.00000 3.00000L4.70228 6.47214L2.85317 0.92705L7.60845 -2.47214L1.76336 -2.42705L0.00000 -8.00000L-1.76336 -2.42705L-7.60845 -2.47214L-2.85317 0.92705L-4.70228 6.47214Z' transform='translate(15,15) scale(1.3,1.3)' fill='url(#gummyStarGrad)' stroke='url(#gummyStarOutline)'></path>
        <text id='gummyStarAura_amount' x='29' y='29' style='font-family:calibri;font-size:12px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='guidingStarAura' style='width:30px;height:30px;display:none'>
    
        <rect width='30' height='30' fill='rgb(150,150,150)'></rect>
        <rect id='guidingStarAura_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <circle cx='15' cy='15' r='13' fill='rgb(200,200,200)' stroke='rgb(90,90,90)' stroke-width='3'></circle>
        <path d='M0 -5L-2 10L2 10Z' fill='rgb(255,0,0)' transform='translate(15,15) rotate(40) translate(0,-8)'></path>
        <path d='M0 -5L-2 10L2 10Z' fill='rgb(0,0,255)' transform='translate(15,15) rotate(180) translate(0,-8)'></path>
        <path d='M0 -5L-2 10L2 10Z' fill='rgb(0,0,0)' transform='translate(15,15) rotate(-40) translate(0,-8)'></path>
        <path d='M0.00000 3.00000L4.70228 6.47214L2.85317 0.92705L7.60845 -2.47214L1.76336 -2.42705L0.00000 -8.00000L-1.76336 -2.42705L-7.60845 -2.47214L-2.85317 0.92705L-4.70228 6.47214Z' transform='translate(15,15) scale(1.3,1.3)' fill='rgb(255,235,100)' stroke='rgb(100,100,100)'></path>
        <text id='guidingStarAura_amount' x='29' y='29' style='font-family:calibri;font-size:12px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='glueBuff' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(255,200,0)'></rect>
        <rect id='glueBuff_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='glueBuff_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        <rect x='33' y='16' rx='7' width='20' height='30' fill='rgb(255,255,255)' stroke='rgb(0,0,0)' stroke-width='1.5' transform='scale(0.4285,0.4285) rotate(15)'></rect>
        <rect x='39.5' y='6' rx='1' width='8' height='10' fill='rgb(255,150,0)' stroke='rgb(0,0,0)' stroke-width='1.5' transform='scale(0.4285,0.4285) rotate(15)'></rect>
        <path fill='rgb(255,30,255)' d='M10 10L20 10L10 25Z' transform='scale(0.4285,0.4285) rotate(15) translate(28,12)'></path>
        <path fill='rgb(25,255,100)' d='M10 10L20 10L10 25Z' transform='scale(0.4285,0.4285) translate(44,62) rotate(195)'></path>
        
    </svg>
    
    <svg id='oilBuff' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(255,200,0)'></rect>
        <rect id='oilBuff_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='oilBuff_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        <path fill='rgb(255,255,120)' stroke='rgb(0,0,0)' stroke-width='1.5' d='M35 55C17 54 17 36 28 30C 31 28 31 21 30 20' transform='scale(0.4285,0.4285)'></path>
        <path fill='rgb(255,255,120)' stroke='rgb(0,0,0)' stroke-width='1.5' d='M35 55C17 54 17 36 28 30C 31 28 31 21 30 20' transform='scale(0.4285,0.4285) scale(-1,1) translate(-70,0)'></path>
        <path fill='rgb(255,255,120)' d='M35 55L31 27L40 27Z' transform='scale(0.4285,0.4285)'></path>
        <rect x='30.5' y='11' width='8' height='10' stroke='rgb(0,0,0)' stroke-width='1.5' rx='3' fill='rgb(200,150,0)' transform='scale(0.4285,0.4285)'></rect>
        <rect x='21' y='38' width='28' height='6'rx='3' fill='rgb(150,70,0)' transform='scale(0.4285,0.4285)'></rect>
        <circle cx='35' cy='41' r='5' fill='rgb(255,255,255)' transform='scale(0.4285,0.4285)'></circle>
        
    </svg>
    
    <svg id='enzymesBuff' style='width:30px;height:30px;display:none'>
        <rect width='30' height='30' fill='rgb(255,200,0)'></rect>
        <rect id='enzymesBuff_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='enzymesBuff_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        <path d='M20 45 C20 30 50 30 50 45M20 45L20 47C20 60 50 60 50 47 L50 45' fill='rgb(100,255,100)' stroke='rgb(0,0,0)' strokeWeight='1.5' transform='scale(0.4285,0.4285) translate(36,-15) scale(0.9,0.9) rotate(25)'></path>
        
        <path d='M20 45 C20 30 50 30 50 45M20 45L20 47C20 60 50 60 50 47 L50 45' fill='rgb(255,255,80)' stroke='rgb(0,0,0)' strokeWeight='1.5' transform='scale(0.4285,0.4285) translate(-11,-9)'></path>
        
        <path d='M20 45 C20 30 50 30 50 45M20 45L20 47C20 60 50 60 50 47 L50 45' fill='rgb(0,0,0,0.2)' transform='scale(0.4285,0.4285) translate(0,11) scale(0.7,0.5)'></path>
        
        <path d='M20 45 C20 30 50 30 50 45M20 45L20 47C20 60 50 60 50 47 L50 45' fill='rgb(0,0,0,0.2)' transform='scale(0.4285,0.4285) translate(52,5) scale(0.7,0.5) rotate(44)'></path>
        
    </svg>
    
    <svg id='redExtractBuff' style='width:30px;height:30px;display:none'>
        <rect width='30' height='30' fill='rgb(255,200,0)'></rect>
        <rect id='redExtractBuff_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='redExtractBuff_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        <path d='M 47 17L 45 42L 37 47L 23 40L 29 16' fill='rgb(0,0,0,0.1)' stroke='rgb(0,0,0)' stroke-width='1.5' transform='scale(0.4285,0.4285) translate(0,7)'></path>
        <path d='M 45 34L 45 42L 37 47L 23 40L 26 30' fill='rgb(200,0,0)' transform='scale(0.4285,0.4285) translate(0,7)'></path>
        <rect x='30' y='3' width='20' height='12' fill='rgb(100,100,100' rx='4' transform='scale(0.4285,0.4285) translate(0,7) rotate(10)'></rect>
        <path d='M 47 12L 45 42L 37 47L 39 10' fill='rgb(0,0,0,0.5)' transform='scale(0.4285,0.4285) translate(0,7)'></path>
        
    </svg>
    
    <svg id='blueExtractBuff' style='width:30px;height:30px;display:none'>
        <rect width='30' height='30' fill='rgb(255,200,0)'></rect>
        <rect id='blueExtractBuff_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='blueExtractBuff_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        <path d='M 47 17L 45 42L 37 47L 23 40L 29 16' fill='rgb(0,0,0,0.1)' stroke='rgb(0,0,0)' stroke-width='1.5' transform='scale(0.4285,0.4285) translate(0,7)'></path>
        <path d='M 45 34L 45 42L 37 47L 23 40L 26 30' fill='rgb(0,0,200)' transform='scale(0.4285,0.4285) translate(0,7)'></path>
        <rect x='30' y='3' width='20' height='12' fill='rgb(100,100,100' rx='4' transform='scale(0.4285,0.4285)  translate(0,7) rotate(10)'></rect>
        <path d='M 47 12L 45 42L 37 47L 39 10' fill='rgb(0,0,0,0.5)' transform='scale(0.4285,0.4285) translate(0,7)'></path>
        
    </svg>
    
    <svg id='tropicalDrinkBuff' style='width:30px;height:30px;display:none'>
        <defs>
            <linearGradient id='strawStripes_' x1='0' x2='0' y1='0' y2='1'>
                <stop offset='10%' stop-color='rgb(235, 112, 235)'/>
                <stop offset='10%' stop-color='rgb(240, 56, 194)'/>
                <stop offset='20%' stop-color='rgb(240, 56, 194)'/>
                <stop offset='20%' stop-color='rgb(235, 112, 235)'/>
                <stop offset='30%' stop-color='rgb(235, 112, 235)'/>
                <stop offset='30%' stop-color='rgb(240, 56, 194)'/>
                <stop offset='40%' stop-color='rgb(240, 56, 194)'/>
                <stop offset='40%' stop-color='rgb(235, 112, 235)'/>
                <stop offset='50%' stop-color='rgb(235, 112, 235)'/>
                <stop offset='50%' stop-color='rgb(240, 56, 194)'/>
                <stop offset='60%' stop-color='rgb(240, 56, 194)'/>
                <stop offset='60%' stop-color='rgb(235, 112, 235)'/>
                <stop offset='70%' stop-color='rgb(235, 112, 235)'/>
                <stop offset='70%' stop-color='rgb(240, 56, 194)'/>
                <stop offset='80%' stop-color='rgb(240, 56, 194)'/>
                <stop offset='80%' stop-color='rgb(235, 112, 235)'/>
                <stop offset='90%' stop-color='rgb(235, 112, 235)'/>
                <stop offset='90%' stop-color='rgb(240, 56, 194)'/>
                
            </linearGradient>
        </defs>
        <rect width='30' height='30' fill='rgb(255,200,0)'></rect>
        <rect id='tropicalDrinkBuff_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='tropicalDrinkBuff_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        <path d='M15 30C15 60 60 60 60 30C 60 15 15 15 15 30' fill='rgb(163, 90, 18)' stroke='rgb(0,0,0)'stroke-width='1.5'  transform='scale(0.4285,0.4285)'></path>
        <path d='M20 30C20 42 55 42 55 30C 55 20 20 20 20 30' fill='rgb(255,255,210)' stroke='rgb(0,0,0)'stroke-width='1.5' transform='scale(0.4285,0.4285)'></path>
        <path d='M 44 37L 39 38L 48 12L 60 7L 61 11L 52 15Z' fill='url(#strawStripes_)' stroke='rgb(0,0,0)'stroke-width='1.5' transform='scale(0.4285,0.4285)'></path>
        
    </svg>
    
    <svg id='purplePotionBuff' style='width:30px;height:30px;display:none'>
        <rect width='30' height='30' fill='rgb(255,200,0)'></rect>
        <rect id='purplePotionBuff_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='purplePotionBuff_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        <path d='M50 50L 20 50L 32 25L32 15L37 15L37 25Z' fill='rgb(0,0,0,0.3)' stroke='rgb(0,0,0)' strokeWeight='1.5' transform='scale(0.4285,0.4285) rotate(15) translate(10,-8)'></path>
        <path d='M50 50L 20 50L 27 38L42.2 38Z' fill='rgb(200,0,200)' transform='scale(0.4285,0.4285) rotate(15) translate(10,-8)'></path>
        <rect x='30.5' y='10' width='8' height='6' transform='scale(0.4285,0.4285) rotate(15) translate(10,-8)'></rect>
        
    </svg>
    
    <svg id='superSmoothieBuff' style='width:30px;height:30px;display:none'>
        <defs>
            <linearGradient id='greenSmoothie' x1='0' x2='0' y1='0' y2='1'>
                <stop offset='46%' stop-color='rgb(0,0,0,0.2)'/>
                
                <stop offset='50%' stop-color='rgb(255,0,50)'/>
                
                <stop offset='60%' stop-color='rgb(255,0,50)'/>
                <stop offset='60%' stop-color='rgb(50,255,50)'/>
                
            </linearGradient>
            
            <linearGradient id='smoothieStraw' x1='0' x2='0' y1='0' y2='1'>
                <stop offset='20%' stop-color='rgb(255,0,0)'/>
                <stop offset='20%' stop-color='rgb(255,255,225)'/>
                <stop offset='40%' stop-color='rgb(255,255,225)'/>
                <stop offset='40%' stop-color='rgb(255,0,0)'/>
                <stop offset='60%' stop-color='rgb(255,0,0)'/>
                <stop offset='60%' stop-color='rgb(255,255,225)'/>
                <stop offset='80%' stop-color='rgb(255,255,225)'/>
                <stop offset='80%' stop-color='rgb(255,0,0)'/>
                
            </linearGradient>
        </defs>
        <rect width='30' height='30' fill='rgb(255,200,0)'></rect>
        <rect id='superSmoothieBuff_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='superSmoothieBuff_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
        <path d='M20 45C20 55 50 55 50 45C65 45 65 20 50 20C50 10 20 10 20 20Z' fill='url(#greenSmoothie)' stroke='rgb(0,0,0)' stroke-width='1.5' transform='scale(0.4285,0.4285) translate(0,3)'></path>
        <path d='M50 40 C 60 40 60 25 50 25M50 40 L 50 25' fill='rgb(225,225,225)' stroke='rgb(0,0,0)' stroke-width='1.5' transform='scale(0.4285,0.4285) translate(0,3)'></path>
        <rect transform='scale(0.4285,0.4285) translate(0,3) rotate(-17)' x='19' y='11' width='5' height='30' stroke='rgb(0,0,0)' stroke-width='1.5' fill='url(#smoothieStraw)'></rect>
        <path d='M50 20C50 25 20 25 20 20' fill='rgb(0,0,0,0)' stroke='rgb(0,0,0)' stroke-width='1.5' transform='scale(0.4285,0.4285) translate(0,3)'></path>
        <circle cx='35' cy='37.4' r='6' fill='rgb(255,0,50)'transform='scale(0.4285,0.4285)'></circle>
        <path d='M36 32C 30 30 30 35 35 35C 37 35 37 39 32 38' stroke='rgb(255,255,255)' stroke-width='2' fill='rgb(0,0,0,0)' transform='scale(0.4285,0.4285) translate(0,3)'></path>
        
    </svg>
    
    <svg id='stingerBuff' style='width:30px;height:30px;display:none'>
        <defs>
            
            <linearGradient id='stingerShade_' x1='-2.3' y1='0' x2='1.0' y2='0.7'>
                <stop offset='40%' stop-color='black'/>
                <stop offset='90%' stop-color='white'/>
                
            </linearGradient>
            
        </defs>
        
        <rect width='30' height='30' fill='rgb(240,0,0)'></rect>
        <rect id='stingerBuff_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <text id='stingerBuff_amount' x='28' y='28' style='font-family:calibri;font-size:13px;' text-anchor='end'></text>
        
        <g transform='scale(0.4285,0.4285) translate(0,-2) scale(0.9,0.9)'>
        <path stroke='black' stroke-width='2' fill='url(#stingerShade_)' d='M30 20L50 50C 73 53 13 71 30 54z'></path><path stroke='black' stroke-width='1' fill='rgb(0,0,0,0)' d='M 48 51 C 42 56 39 55 33 56'></path></g>
        
    </svg>
    
    <svg id='bubbleBloat' style='width:30px;height:30px;display:none;'>
        
        <rect width='30' height='30' fill='rgb(0,0,200)'></rect>
        <rect id='bubbleBloat_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <circle cx='10' cy='6' r='2' fill='rgb(100,100,255)'></circle><circle cx='19' cy='10' r='3' fill='rgb(100,100,255)'></circle><circle cx='11' cy='16' r='3' fill='rgb(100,100,255)'></circle><circle cx='19' cy='17' r='2' fill='rgb(100,100,255)'></circle><circle cx='15' cy='26' r='5' fill='rgb(100,100,255)'></circle>
        <text id='bubbleBloat_amount' x='29' y='29' style='font-family:calibri;font-size:12px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='gummyBall' style='width:30px;height:30px;display:none;'>
        
        <rect width='30' height='30' fill='rgb(30,255,100)'></rect>
        <rect id='gummyBall_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <!--30,255,100  255,50,255-->
        <circle cx='15' cy='15' r='7' fill='rgb(255,50,255)'></circle>
        <text id='gummyBall_amount' x='29' y='29' style='font-family:calibri;font-size:12px;' text-anchor='end'></text>
        
    </svg>
    
    <svg id='gummyBallCombo' style='width:30px;height:30px;display:none;'>
        
        <rect width='30' height='30' fill='rgb(30,255,100)'></rect>
        <rect id='gummyBallCombo_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.45'></rect>
        <circle cx='10' cy='10' r='7' fill='rgb(255,50,255)'></circle><circle cx='16' cy='16' r='4' fill='rgb(255,50,255)'></circle><circle cx='20' cy='20' r='2' fill='rgb(255,50,255)'></circle>
        <text id='gummyBallCombo_amount' x='29' y='29' style='font-family:calibri;font-size:12px;' text-anchor='end'></text>
        
    </svg>
    
   <svg id='redBombSync' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(0,0,0)'></rect>
        <text id='redBombSync_amount' x='29' y='29' style='font-family:calibri;font-size:12px;' text-anchor='end'></text>
        
        <circle cx='15' cy='17.5' r='6' fill='red'></circle>
        <rect x='11.25' y='11' width='7.5' height='3' fill='red'></rect>
        <path fill='white' d='M17.5 11L18.75 11L18.75 12.5C 25 18 18.4 25.1 13 23.2C 19 17 11 17 17.5 11'></path>
        <path stroke='red' d='M15 11C 15 6 20 11 20 6' stroke-width='1'></path>
        <rect id='redBombSync_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.6'></rect>
            
    </svg>
    
    <svg id='blueBombSync' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(0,0,0)'></rect>
        
        <text id='blueBombSync_amount' x='29' y='29' style='font-family:calibri;font-size:12px;' text-anchor='end'></text>
        
        <circle cx='15' cy='17.5' r='6' fill='blue'></circle>
        <rect x='11.25' y='11' width='7.5' height='3' fill='blue'></rect>
        <path fill='white' d='M17.5 11L18.75 11L18.75 12.5C 25 18 18.4 25.1 13 23.2C 19 17 11 17 17.5 11'></path>
        <path stroke='blue' d='M15 11C 15 6 20 11 20 6' stroke-width='1'></path>
        <rect id='blueBombSync_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.6'></rect>
            
    </svg>

    <svg id='corruption' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(0,0,0)'></rect>
        
        <text x='15.5' y='25' style='font-size:39px;' text-anchor='middle' fill='rgb(255,0,200)'>☺</text>

        <text id='corruption_amount' x='29' y='29' style='font-family:calibri;font-size:12px' text-anchor='end' fill='rgb(255,255,255)'></text>
        
        <rect id='corruption_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.6'></rect>
        
    </svg>

    <svg id='redJellyBean' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(234,102,102)'></rect>

        <text id='redJellyBean_amount' x='29' y='29' style='font-family:calibri;font-size:12px' text-anchor='end' fill='rgb(0,0,0)'></text>
        
        <rect id='redJellyBean_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.6'></rect>
        
        <g transform='scale(0.85,0.85) translate(23,24)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(230, 32, 32)' d='M-15 -15 C -20 4 4 8 5 0C 9 -3 -8 -24 -15 -15'></path>
        <path stroke='rgb(255,255,255,0.7)' stroke-width='3' fill='rgb(0,0,0,0)' d='M-11 -7 C -15 -14 -5 -16 -8 -11M-5 0 C 1 1 4 0 0 -6'></path>
        <circle cx='-7' cy='-3' r='1.8' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-2' cy='0' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-10' cy='-8' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        </g>
    </svg>
    
    <svg id='whiteJellyBean' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(222,222,222)'></rect>

        <text id='whiteJellyBean_amount' x='29' y='29' style='font-family:calibri;font-size:12px' text-anchor='end' fill='rgb(0,0,0)'></text>
        
        <rect id='whiteJellyBean_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.6'></rect>
        
        <g transform='scale(0.85,0.85) translate(23,24)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(190,190,190)' d='M-15 -15 C -20 4 4 8 5 0C 9 -3 -8 -24 -15 -15'></path>
        <path stroke='rgb(255,255,255,0.7)' stroke-width='3' fill='rgb(0,0,0,0)' d='M-11 -7 C -15 -14 -5 -16 -8 -11M-5 0 C 1 1 4 0 0 -6'></path>
        <circle cx='-7' cy='-3' r='1.8' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-2' cy='0' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-10' cy='-8' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        </g>
    </svg>
    
    <svg id='blueJellyBean' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(103, 174, 245)'></rect>

        <text id='blueJellyBean_amount' x='29' y='29' style='font-family:calibri;font-size:12px' text-anchor='end' fill='rgb(0,0,0)'></text>
        
        <rect id='blueJellyBean_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.6'></rect>
        
        <g transform='scale(0.85,0.85) translate(23,24)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(49, 82, 247)' d='M-15 -15 C -20 4 4 8 5 0C 9 -3 -8 -24 -15 -15'></path>
        <path stroke='rgb(255,255,255,0.7)' stroke-width='3' fill='rgb(0,0,0,0)' d='M-11 -7 C -15 -14 -5 -16 -8 -11M-5 0 C 1 1 4 0 0 -6'></path>
        <circle cx='-7' cy='-3' r='1.8' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-2' cy='0' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-10' cy='-8' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        </g>
    </svg>
    
    <svg id='pinkJellyBean' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(255, 178, 238)'></rect>

        <text id='pinkJellyBean_amount' x='29' y='29' style='font-family:calibri;font-size:12px' text-anchor='end' fill='rgb(0,0,0)'></text>
        
        <rect id='pinkJellyBean_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.6'></rect>
        
        <g transform='scale(0.85,0.85) translate(23,24)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(227, 88, 195)' d='M-15 -15 C -20 4 4 8 5 0C 9 -3 -8 -24 -15 -15'></path>
        <path stroke='rgb(255,255,255,0.7)' stroke-width='3' fill='rgb(0,0,0,0)' d='M-11 -7 C -15 -14 -5 -16 -8 -11M-5 0 C 1 1 4 0 0 -6'></path>
        <circle cx='-7' cy='-3' r='1.8' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-2' cy='0' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-10' cy='-8' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        </g>
    </svg>
    
    <svg id='brownJellyBean' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(222, 144, 60)'></rect>

        <text id='brownJellyBean_amount' x='29' y='29' style='font-family:calibri;font-size:12px' text-anchor='end' fill='rgb(0,0,0)'></text>
        
        <rect id='brownJellyBean_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.6'></rect>
        
        <g transform='scale(0.85,0.85) translate(23,24)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(140, 83, 23)' d='M-15 -15 C -20 4 4 8 5 0C 9 -3 -8 -24 -15 -15'></path>
        <path stroke='rgb(255,255,255,0.7)' stroke-width='3' fill='rgb(0,0,0,0)' d='M-11 -7 C -15 -14 -5 -16 -8 -11M-5 0 C 1 1 4 0 0 -6'></path>
        <circle cx='-7' cy='-3' r='1.8' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-2' cy='0' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-10' cy='-8' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        </g>
    </svg>
    
    <svg id='greenJellyBean' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(118, 207, 93)'></rect>

        <text id='greenJellyBean_amount' x='29' y='29' style='font-family:calibri;font-size:12px' text-anchor='end' fill='rgb(0,0,0)'></text>
        
        <rect id='greenJellyBean_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.6'></rect>
        
        <g transform='scale(0.85,0.85) translate(23,24)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(48, 145, 19)' d='M-15 -15 C -20 4 4 8 5 0C 9 -3 -8 -24 -15 -15'></path>
        <path stroke='rgb(255,255,255,0.7)' stroke-width='3' fill='rgb(0,0,0,0)' d='M-11 -7 C -15 -14 -5 -16 -8 -11M-5 0 C 1 1 4 0 0 -6'></path>
        <circle cx='-7' cy='-3' r='1.8' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-2' cy='0' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-10' cy='-8' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        </g>
    </svg>
    
    <svg id='blackJellyBean' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(148, 148, 148)'></rect>

        <text id='blackJellyBean_amount' x='29' y='29' style='font-family:calibri;font-size:12px' text-anchor='end' fill='rgb(0,0,0)'></text>
        
        <rect id='blackJellyBean_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.6'></rect>
        
        <g transform='scale(0.85,0.85) translate(23,24)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(50,50,50)' d='M-15 -15 C -20 4 4 8 5 0C 9 -3 -8 -24 -15 -15'></path>
        <path stroke='rgb(255,255,255,0.7)' stroke-width='3' fill='rgb(0,0,0,0)' d='M-11 -7 C -15 -14 -5 -16 -8 -11M-5 0 C 1 1 4 0 0 -6'></path>
        <circle cx='-7' cy='-3' r='1.8' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-2' cy='0' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-10' cy='-8' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        </g>
    </svg>
    
    <svg id='yellowJellyBean' style='width:30px;height:30px;display:none'>
        
        <rect width='30' height='30' fill='rgb(255, 240, 171)'></rect>

        <text id='yellowJellyBean_amount' x='29' y='29' style='font-family:calibri;font-size:12px' text-anchor='end' fill='rgb(0,0,0)'></text>
        
        <rect id='yellowJellyBean_cooldown' width='30' height='0' style='fill:rgb(0,0,0);' opacity='0.6'></rect>
        
        <g transform='scale(0.85,0.85) translate(23,24)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(255, 230, 0)' d='M-15 -15 C -20 4 4 8 5 0C 9 -3 -8 -24 -15 -15'></path>
        <path stroke='rgb(255,255,255,0.9)' stroke-width='3' fill='rgb(0,0,0,0)' d='M-11 -7 C -15 -14 -5 -16 -8 -11M-5 0 C 1 1 4 0 0 -6'></path>
        <circle cx='-7' cy='-3' r='1.8' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-2' cy='0' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-10' cy='-8' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        </g>
    </svg>
    
</div>

<div id='honeyAndPollenAmount' style='position:fixed;margin:0px;top:0px;left:51.5%;width:600px;z-index:-1;transform:translate(-50%,0px);'>
    
    <svg style='margin:0px;width:600px;height:35px;'>
        
        <rect x='5' width='510' height='30' fill='rgb(210,210,210)' rx='5'></rect>
        <rect x='313' y='3' width='196' height='23' fill='rgb(120,120,120)' rx='2'></rect>
        <rect x='62' y='3' width='196' height='23' fill='rgb(120,120,120)' rx='2'></rect>
        <text x='10' y='20' style='font-family:trebuchet ms;font-size:16px;'>Honey:</text>
        <text x='262' y='20' style='font-family:trebuchet ms;font-size:16px;'>Pollen:</text>
        
        <rect id='capacityBar' x='313' y='3' width='0' height='23' style='fill:rgb(0,255,0);'></rect>
        
        <text id='honeyAmount2' x='68' y='20' style='font-family:trebuchet ms;font-size:16px;' fill='rgb(255, 226, 8)' stroke='rgb(0,0,0)' stroke-width='2'>0</text>
        <text id='honeyAmount' x='68' y='20' style='font-family:trebuchet ms;font-size:16px;' fill='rgb(255, 226, 8)'>0</text>
        
        <text id='pollenAmount2' x='319' y='20' style='font-family:trebuchet ms;font-size:16px;' fill='rgb(255,255,255)' stroke='rgb(0,0,0)' stroke-width='2'>0</text>
        <text id='pollenAmount' x='319' y='20' style='font-family:trebuchet ms;font-size:16px;' fill='rgb(255,255,255)'>0</text>
        
        <rect x='520' y='10' width='72' height='10' style='fill:rgb(100,100,100);' rx='2'></rect>
        <rect id='healthBar' x='520' y='10' width='72' height='10' style='fill:rgb(0,255,0);' rx='2'></rect>
        
    </svg>
</div>

<div id='hoverText' style='background-color:rgb(0,0,0);position:fixed;color:rgb(255,255,255);display:none;text-align:center;padding:5px;z-index:1'></div>


<div id='mainMenu' style='position:fixed;z-index:1;background-color:rgb(255,255,255);top:0px;left:0px;bottom:0px;right:0px;'>
    
    <canvas id='thumbnailCanv' style='position:fixed;left:50%;top:50%;z-index:-1;transform:translate(-50%,-50%);'></canvas>

    <div style='position:fixed;left:50%;top:10%;text-align:center;font-size:45px;text-align:center;background-color:rgb(255,255,255,0.6);border-radius:15px;padding:5px;padding-left:25px;padding-right:25px;transform:translate(-50%,-50%);width:5000px;backdrop-filter:blur(10px)'>Bee Swarm Simulator</div>
    
    <button id='mainPlay' style='position:fixed;left:50%;top:50%;width:300px;height:70px;background-color:rgb(157, 255, 150);transform:translate(-50%,-130%);font-family:comic sans ms;font-size:35px'>Play</button>

    <button id='mainInfo' style='position:fixed;left:50%;top:50%;width:300px;height:70px;background-color:rgb(248, 129, 129);transform:translate(-50%,0%);font-family:comic sans ms;font-size:23px'>Instructions & Info</button>

    <button id='mainNew' style='position:fixed;left:50%;top:50%;width:300px;height:70px;background-color:rgb(248, 255, 150);transform:translate(-50%,130%);font-family:comic sans ms;font-size:23px'>Open In New Window</button>
    
</div>

<div id='mainInfoMenu' style='position:fixed;z-index:1;top:0px;left:0px;background-color:rgb(255,255,255);bottom:0px;right:0px;display:none;overflow-y:auto'>

    <canvas id='info_thumbnailCanvCopy' style='position:fixed;left:50%;top:50%;z-index:-1;transform:translate(-50%,-50%);filter:blur(10px)'></canvas>

    <div style='margin-top:20px;font-size:45px;text-align:center;'>Instructions & Info</div>
    
    <button id='info_mainBack' style='margin-left:80px;margin-top:-70px;width:120px;height:60px;background-color:rgb(174, 216, 255);transform:translate(-50%,-50%);font-family:comic sans ms;font-size:25px'>Back</button>

    <div style='margin-left:10px;font-size:19px;text-align:center;user-select:text'>Welcome to Bee Swarm Simulator! In this game, you collect pollen from flowers and make honey.<br><br>But you don't do it alone... You are the leader of your own personal swarm of bees!<br><br>Open your inventory by clicking on the egg button. Click on the egg to select it, and then click a slot in your hive to hatch it.<br><br>With your bees, go into the fields to collect pollen. You can collect pollen by swinging your tool or when your bee gathers. After your bag becomes full, go back to your hive and your bees will convert the pollen into honey.<br><br>Talk to bears and NPCs around the mountain and complete their quests for rewards!<br><br>Expand your hive by buying more eggs and level up bees with treats. Upgrade your tools to make more honey. Defeat monsters around the map for loot. Collect rare ingredients to craft powerful gear!<br><br><br><br><br>------------------------------------------<br><br><br><b>Not all features are accurate to the original. Many changes include dialogue, quests, gear stats, loot drops, certain calculations, improved RNG rates, and singleplayer. All graphics are recreations of the original, not copied. Gameplay is meant to be accelerated from the original through the use of decreased item & bee rarity + easier & more rewarding quests. Audio is non-existent for this game.</b><br><br>GitHub release version: v2.0.0<br><br>Made by Dat<br><br>Original made by Onett<br><br>Some info from the BSS Fandom Wiki<br><br>Bugtesters: HB_The_Pencil, Astro, and more.<br><br><br></div>

</div>

<div id='mainSelectMenu' style='position:fixed;z-index:1;top:0px;left:0px;background-color:rgb(255,255,255);bottom:0px;right:0px;display:none;overflow-y:auto'>

    <canvas id='select_thumbnailCanvCopy' style='position:fixed;left:50%;top:50%;z-index:-1;transform:translate(-50%,-50%);filter:blur(10px)'></canvas>

    <div style='margin-top:20px;font-size:45px;text-align:center;'>Saved Games</div>
    
    <button id='select_mainBack' style='margin-left:80px;margin-top:-70px;width:120px;height:60px;background-color:rgb(174, 216, 255);transform:translate(-50%,-50%);font-family:comic sans ms;font-size:25px'>Back</button>

    <div style='width:100%;height:50px;'>

        <button id='createNewGame' style='position:absolute;left:50%;width:200px;height:50px;background-color:rgb(225,225,225);transform:translate(-210px,0px);font-family:comic sans ms;font-size:23px;'>New Game</button>

        <button id='createImportedGame' style='position:absolute;left:50%;width:200px;height:50px;background-color:rgb(225,225,225);transform:translate(10px,0px);font-family:comic sans ms;font-size:23px;'>Import Game</button>

    </div>

    <br><br>

    <div id='savedGames'></div>

</div>

<div id='UIBar' style='position:absolute;margin:0px;z-index:0;width:170px;height:30px;background-color:rgba(195,195,195);margin-top:103px;padding:2px;border-radius:2px'>
    
    <svg id='inventoryButton' style='width:30px;height:30px;transition-duration:0.4s;'>
        
        <path fill='rgb(255,255,255)' d='M15 27C0 28 5 5 15 5ZM15 27C30 28 25 5 15 5'></path>
        
        <path stroke='rgb(195,195,195)' fill='rgb(0,0,0,0)' stroke-width='4' d='M5 15L 10 12L15 18L20 13L25 15'></path>
        
    </svg>
    
    <svg id='questButton' style='width:30px;height:30px;transition-duration:0.4s;'>
        
        <path fill='rgb(255,255,255)' d='M 0 6 L 10 2L 20 6L 30 2L 30 24L 20 28L 10 24 L 0 28' transform='translate(3,0) scale(0.8,1)'></path>
        <text fill='rgb(195,195,195)' stroke='rgb(195,195,195)' stroke-width='0.3' x='10' y='21' style='font-family:arial;font-size:18px'>?</text>
        
    </svg>

    <svg id='beesButton' style='width:30px;height:30px;transition-duration:0.4s;'>
        
        <rect fill='rgb(255,255,255)' x='2.5' y='2.5' width='25' height='25' rx='3'></rect>
        
        <path fill='rgb(255,255,255,0)' stroke='rgb(195,195,195)' stroke-width='3' d='M10 7 L 10 14M20 7 L 20 14M11 18 L 15 22L 19 18'></path>
        
    </svg>
    
    <svg id='settingsButton' style='width:30px;height:30px;transition-duration:0.4s;'>
        
        <circle cx='15' cy='15' r='8' fill='rgb(255,255,255)'></circle>
        <rect x='-3' y='-12' width='6' height='10' fill='rgb(255,255,255)' transform='translate(15,15) rotate(0)'></rect>
        <rect x='-3' y='-12' width='6' height='10' fill='rgb(255,255,255)' transform='translate(15,15) rotate(45)'></rect>
        <rect x='-3' y='-12' width='6' height='10' fill='rgb(255,255,255)' transform='translate(15,15) rotate(90)'></rect>
        <rect x='-3' y='-12' width='6' height='10' fill='rgb(255,255,255)' transform='translate(15,15) rotate(135)'></rect>
        <rect x='-3' y='-12' width='6' height='10' fill='rgb(255,255,255)' transform='translate(15,15) rotate(180)'></rect>
        <rect x='-3' y='-12' width='6' height='10' fill='rgb(255,255,255)' transform='translate(15,15) rotate(225)'></rect>
        <rect x='-3' y='-12' width='6' height='10' fill='rgb(255,255,255)' transform='translate(15,15) rotate(270)'></rect>
        <rect x='-3' y='-12' width='6' height='10' fill='rgb(255,255,255)' transform='translate(15,15) rotate(315)'></rect>
        <circle cx='15' cy='15' r='3' fill='rgb(195,195,195)'></circle>
        
    </svg>
    
    <svg id='beequipButton' style='width:30px;height:30px;transition-duration:0.4s;'>
        
        <path fill='rgb(255,255,255)' transform='scale(0.056,0.056)' d='M511.883,298.395c-2.781-27.281-54.391-46.141-129.406-51.844c-7.172-42.047-15.469-90.563-17.891-103.75
		c-5.563-30.203-45.344-47.094-74.891-25.313c-15.5,11.422-29.359,12.234-36.703,12.234s-15.5,1.625-36.703-12.234
		c-30.719-20.094-69.328-4.891-74.875,25.313c-2.969,16.109-14.688,84.844-22.391,130.203
		C45.211,293.817-2.711,323.114,0.117,350.723c4.25,41.625,122.266,63.671,263.578,49.218
		C405.039,385.488,516.148,340.036,511.883,298.395z M132.289,308.348l8.156-42.406c0,0,145.188,22.828,226.75-19.578l8.156,35.891
		C375.352,282.254,287.258,337.708,132.289,308.348z'></path>
        
    </svg>
    
</div>

<div class='uiPage' style='position:absolute;margin:0px;z-index:-2;overflow-y:auto;overflow-x:hidden;width:200px;height:455px;background-color:rgba(195,195,195,0.9);margin-top:140px;display:none;padding:3px;'>
    
    <div id='noItemsMessage' style='text-align:center;font-size:17px;color:rgb(0,0,0,0.5)'><br>(You have no items)</div>

    <svg id='honey' style='width:200px;height:70px;cursor:pointer'>
        
        <text></text>
        <path fill='rgb(255, 207, 13)' stroke='rgb(0,0,0)' stroke-width='2' d='M32 20C 54 38 50 59 33 54C 20 44 41 34 32 20' transform='scale(1.06,1.06) translate(-6,-7)'></path>
    </svg>
    
    <svg id='ticket' style='width:200px;height:70px;cursor:pointer;border-radius:5px;'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='134' y='19' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Ticket</text>
        <text x='132' y='39' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Used to activate and</text>
        <text x='132' y='52' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>purchase special things!</text>
        <text id='ticket_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(35,35) scale(1,0.6)'>
        <path fill='rgb(255, 200, 5)' stroke='black' stroke-width='2' d='M -15 -20L 15 -20C16 -15 15 -15 20 -15L20 15C15 15 15 15 15 20L-15 20C-15 15 -15 15 -20 15L-20 -15C-15 -15 -15 -15 -15 -20'></path>
        <path stroke='rgb(255,0,0)' fill='rgb(0,0,0,0)' stroke-width='1' d='M-15 -11L15 -11L15 11L-15 11L-15 -11M11 -11L11 11M-11 -11L-11 11'></path>
        <path stroke='rgb(255,0,0)' fill='rgb(0,0,0,0)' stroke-width='2' d='M-3 -7L-3 -0M3 -7L3 -0M-3 3L0 7L3 3'></path>
        </g>
    </svg>
	
    <svg id='gumdrops' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Gumdrops</text>
        <text x='132' y='35' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Use while standing in a</text>
        <text x='132' y='50' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>field to cover flowers in</text>
        <text x='132' y='65' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>goo, giving bonus honey.</text>
        <text id='gumdrops_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        <path fill='rgb(255,100,255)' stroke='black' stroke-width='2' d='M30 60C 26 25 64 25 60 60C55 67 35 67 30 60' transform='translate(10,-6) scale(0.75,0.75)'></path>
        <path fill='rgb(0,255,100)' stroke='black' stroke-width='2' d='M30 60C 26 25 64 25 60 60C55 67 35 67 30 60' transform='translate(-7,3) scale(0.75,0.75)'></path><circle cx='49' cy='36' r='0.25' stroke='rgb(255,255,255)'></circle><circle cx='43' cy='24' r='0.25' stroke='rgb(255,255,255)'></circle><circle cx='50' cy='29' r='0.25' stroke='rgb(255,255,255)'></circle><circle cx='42' cy='32' r='0.25' stroke='rgb(255,255,255)'></circle><circle cx='41' cy='39' r='0.25' stroke='rgb(255,255,255)'></circle><circle cx='38' cy='27' r='0.25' stroke='rgb(255,255,255)'></circle><circle cx='25' cy='32' r='0.75' stroke='rgb(255,255,255)'></circle><circle cx='33' cy='40' r='0.75' stroke='rgb(255,255,255)'></circle><circle cx='28' cy='46' r='0.75' stroke='rgb(255,255,255)'></circle><circle cx='21' cy='40' r='0.75' stroke='rgb(255,255,255)'></circle><circle cx='27' cy='38' r='0.75' stroke='rgb(255,255,255)'></circle>
    </svg>
    
    <svg id='coconut' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Coconut</text>
        <text x='133' y='35' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Drops a coconut into the</text>
        <text x='133' y='50' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>field. Catch it to convert</text>
        <text x='133' y='65' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>pollen into honey tokens.</text>
        <text id='coconut_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        <circle fill='rgb(160,80,20)' cx='35' cy='35' r='15' stroke='black' stroke-width='2'></circle><circle fill='rgb(70,20,0)' cx='43' cy='35' r='2.5'></circle><circle fill='rgb(70,20,0)' cx='36' cy='27' r='2.5'></circle><circle fill='rgb(70,20,0)' cx='33' cy='37' r='2.5'></circle>
    </svg>
    
    <svg id='stinger' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Stinger</text>
        <text x='133' y='40' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Grants your bees x1.5</text>
        <text x='133' y='56' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'> attack for 30 seconds.</text>
        <text id='stinger_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        <g transform='translate(0,-2) scale(0.9,0.9)'>
        <path stroke='black' stroke-width='2' fill='rgb(255,255,255)' d='M30 20L50 50C 73 53 13 71 30 54z'></path><path stroke='black' stroke-width='1' fill='rgb(0,0,0,0)' d='M 48 51 C 42 56 39 55 33 56'></path>
        <path stroke='rgb(0,0,0,0.1)' stroke-width='6' fill='rgb(255,255,255,0)' d='M 31 24L33 55L26 60'></path><path stroke='black' stroke-width='1' fill='rgb(0,0,0,0)' d='M 48 51 C 42 56 39 55 33 56'></path>
        </g>
    </svg>

    <svg id='microConverter' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='132' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Micro Converter</text>
        <text x='133' y='41' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Instanly converts all</text>
        <text x='133' y='55' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'> pollen in your bag.</text>
        <text id='microConverter_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='scale(0.65,0.65) translate(17,23)'>
        <path stroke='black' stroke-width='1.5' fill='rgb(255,255,255)' d='M 15 20 C 2 20 2 50 15 50L 23 50 C 9 50 9 20 23 20z'></path>
        <circle stroke='black' stroke-width='0.5' fill='rgb(235,235,235)' cx='21' cy='20' r='8.5' transform='scale(1,1.77)'></circle>
        
        <path stroke='black' stroke-width='1' fill='rgb(0,205,0)' d='M 15 20 C 2 20 2 50 15 50L 53 50 C 45 50 45 20 53 20z' transform='translate(11,7.5) scale(0.8,0.8)'></path>
        
        <g transform='translate(39,1)'>
        <path stroke='black' stroke-width='1.5' fill='rgb(255,255,255)' d='M 15 20 C 2 20 2 50 15 50L 23 50 C 9 50 9 20 23 20z'></path>
        <circle stroke='black' stroke-width='0.5' fill='rgb(235,235,235)' cx='21' cy='20' r='8.5' transform='scale(1,1.77)'></circle>
        </g>
        
        <path stroke='rgb(100,100,100)' stroke-width='3' fill='rgb(0,205,0,0)' d='M 5 38 L 13 6L 57 6L 61 39'></path>
        <path stroke='rgb(130, 243, 255)' stroke-width='4' fill='rgb(0,205,0,0)' d='M0 0L5 -5L10 5L15 -5L20 5L25 -5L30 5L35 -5L40 5L45 -5L50 5L55 -5' transform='translate(6,5)'></path>
        <path fill='rgb(255, 207, 13)' stroke='rgb(0,0,0)' stroke-width='2.5' d='M32 20C 54 38 50 59 33 54C 20 44 41 34 32 20' transform='scale(0.6,0.5) translate(18,-27)'></path>
        
        </g>
        
        
    </svg>
    
    <svg id='honeysuckle' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='132' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Honeysuckle</text>
        <text x='133' y='39' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Makes bees convert</text>
        <text x='133' y='53' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>pollen when your bag</text>
        <text x='133' y='67' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>is full.</text>
        <text id='honeysuckle_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform=' scale(0.567,0.675) translate(51,28)'>
        
        <path stroke='rgb(0,150,0)' stroke-width='7' d='M 29 50C 36 34 28 27 20 18' fill='rgb(0,0,0,0)'></path>
        
        <path stroke='black' stroke-width='1.5' d='M20 20C 12 34 -14 22 -7 24C -5 12 7 8 10 12' fill='rgb(255,245,0)'></path>
        <path stroke='black' stroke-width='1.5' d='M20 20C 12 34 -14 22 -7 24C -5 12 7 8 10 12' fill='rgb(255,245,0)' transform='translate(-9,29) rotate(5) scale(1,-1)'></path>
        <path stroke='black' stroke-width='1.5' d='M20 20C 12 34 -14 22 -7 24C -5 12 7 8 10 12' fill='rgb(255,245,0)' transform='translate(-4,22) rotate(36) scale(1,-1)'></path>
        
        <path stroke='rgb(200,170,0)' stroke-width='4' d='M -23 13C 2 18 -4 10 1 14' fill='rgb(0,0,0,0)'></path>
        <path stroke='rgb(200,170,0)' stroke-width='4' d='M -19 0C 2 2 -4 2 1 14' fill='rgb(0,0,0,0)'></path>
        
        </g>
        
        
    </svg>
    
    <svg id='whirligig' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='132' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Whirligig</text>
        <text x='133' y='39' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Swiftly carries you and</text>
        <text x='133' y='53' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>your bees back to</text>
        <text x='133' y='67' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>the hive!</text>
        <text id='whirligig_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(-7,12) rotate(-20)'>
        
        <path stroke='rgb(58, 143, 27)' stroke-width='3' d='M 35 58C 33 55 31 48 34 45' fill='rgb(0,0,0,0)'></path>
        
        <path stroke='black' stroke-width='1.5' d='M 34 44C 11 -10 8 45 34 44' fill='rgb(128, 89, 30)'></path>
        <path stroke='black' stroke-width='1.5' d='M 34 44C 48 -17 61 45 34 44' fill='rgb(128, 89, 30)'></path>
        
        </g>
        
        
    </svg>

    <svg id='fieldDice' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Field Dice</text>
        <text x='133' y='36' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Boosts a random field,</text>
        <text x='133' y='50' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>granting +100% pollen</text>
        <text x='133' y='64' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>for 15 minutes.</text>
        <text id='fieldDice_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(0,-3)'>
        <g transform='translate(0,-4)'>
        
        <path fill='rgb(255,255,255)' stroke='black' stroke-width='3' d='M35 20L20 30L20 50L35 60L50 50L50 30z'></path>
        
        <path fill='rgb(250,250,250)' d='M35 20L20 30L35 40L50 30z'></path>
        <path fill='rgb(225,225,225)' d='M20 30L20 50L35 60L35 40z'></path>
        <path fill='rgb(205,205,205)' d='M50 30L50 50L35 60L35 40z'></path>
        
        </g>
        
        <g transform='scale(1,0.9) translate(37,14) rotate(45) scale(0.65,0.65)'>
        
        <path d='M 15 22L 15 28' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='5'></path>
        
        <path d='M 15 22L 15 27' fill='rgb(0,0,0,0)' stroke='rgb(171, 118, 19)' stroke-width='3'></path>
        
        <path d='M 15 7L 11 12L 13 12L 8 18L 12 18L 7 23L15 23' fill='rgb(0,160,0)' stroke='#57b5b1' stroke-width='1'></path>
        <path d='M 15 7L 11 12L 13 12L 8 18L 12 18L 7 23L15 23' fill='rgb(0,160,0)' stroke='#57b5b1' stroke-width='1' transform='translate(-15,0) scale(-1,1) translate(-44.75,0)'></path></g>
    
        <g transform='scale(0.9,1.2) translate(26,26) rotate(25) scale(0.5,0.5)'>
        <path d='M 13 24C 2 12 2 4 15 5M15 5C 29 5 22 18 12 24' fill='rgb(255,40,60)' stroke='#57b5b1' stroke-width='2'></path>
        
        <g transform='rotate(62) scale(0.7,1) translate(1,-14)'>
        <path d='M 6.8 4C 13 8 16 5 19 1M 15 -2C 13 8 13 5 14 10' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='4'></path>
        <path d='M 8 5C 13 8 16 5 18 2M 15 -1C 13 8 13 5 14 9' fill='rgb(0,0,0,0)' stroke='rgb(0,170,0)' stroke-width='2'></path>
        </g>
        
        <circle cx='10' cy='12' r='1' fill='rgb(255,255,40)'></circle><circle cx='18' cy='10' r='1' fill='rgb(255,255,40)'></circle><circle cx='13' cy='19' r='1' fill='rgb(255,255,40)'></circle><circle cx='15' cy='14' r='1' fill='rgb(255,255,40)'></circle>
        </g>
        
        <g transform='translate(38,32) rotate(9) scale(0.5,0.7) scale(0.75,0.75)'>
        <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15)'></path>
        <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(60)'></path>
        <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(120)'></path>
        <circle cx='15' cy='15' r='5' fill='rgb(255,255,0)'></circle>
        </g>
        </g>
        
    </svg>
    
    <svg id='smoothDice' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Smooth Dice</text>
        <text x='133' y='36' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Boosts 2 random fields,</text>
        <text x='133' y='50' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>granting +200% pollen</text>
        <text x='133' y='64' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>for 15 minutes.</text>
        <text id='smoothDice_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(14,0) rotate(-5) scale(0.85,0.85)'>
            
            <g transform='translate(0,-4) scale(1,1)'>
            
            <path fill='rgb(250,250,250)' stroke='black' stroke-width='3' d='M35 20L20 30L20 50L35 60L50 50L50 30z'></path>
            
            <path fill='rgb(245, 245, 117)' d='M35 20L20 30L35 40L50 30z'></path>
            <path fill='rgb(219, 219, 103)' d='M20 30L20 50L35 60L35 40z'></path>
            <path fill='rgb(189, 189, 85)' d='M50 30L50 50L35 60L35 40z'></path>
            
            </g>
            
            <g transform='scale(1,0.9) translate(37,14) rotate(45) scale(0.65,0.65)'>
            
            <path d='M 15 22L 15 28' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='5'></path>
            
            <path d='M 15 22L 15 27' fill='rgb(0,0,0,0)' stroke='rgb(171, 118, 19)' stroke-width='3'></path>
            
            <path d='M 15 7L 11 12L 13 12L 8 18L 12 18L 7 23L15 23' fill='rgb(0,160,0)' stroke='#57b5b1' stroke-width='1'></path>
            <path d='M 15 7L 11 12L 13 12L 8 18L 12 18L 7 23L15 23' fill='rgb(0,160,0)' stroke='#57b5b1' stroke-width='1' transform='translate(-15,0) scale(-1,1) translate(-44.75,0)'></path></g>
        
            <g transform='scale(0.9,1.2) translate(26,26) rotate(25) scale(0.5,0.5)'>
            <path d='M 13 24C 2 12 2 4 15 5M15 5C 29 5 22 18 12 24' fill='rgb(255,40,60)' stroke='#57b5b1' stroke-width='2'></path>
            
            <g transform='rotate(62) scale(0.7,1) translate(1,-14)'>
            <path d='M 6.8 4C 13 8 16 5 19 1M 15 -2C 13 8 13 5 14 10' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='4'></path>
            <path d='M 8 5C 13 8 16 5 18 2M 15 -1C 13 8 13 5 14 9' fill='rgb(0,0,0,0)' stroke='rgb(0,170,0)' stroke-width='2'></path>
            </g>
            
            <circle cx='10' cy='12' r='1' fill='rgb(255,255,40)'></circle><circle cx='18' cy='10' r='1' fill='rgb(255,255,40)'></circle><circle cx='13' cy='19' r='1' fill='rgb(255,255,40)'></circle><circle cx='15' cy='14' r='1' fill='rgb(255,255,40)'></circle>
            </g>
            
            <g transform='translate(38,32) rotate(9) scale(0.5,0.7) scale(0.75,0.75)'>
            <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15)'></path>
            <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(60)'></path>
            <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(120)'></path>
            <circle cx='15' cy='15' r='5' fill='rgb(255,255,0)'></circle>
            </g>
        </g>
        
        <g transform='translate(-1,6) rotate(5) scale(0.85,0.85)'>
            <g transform='translate(0,-4) scale(1,1)'>
            
            <path fill='rgb(250,250,250)' stroke='black' stroke-width='3' d='M35 20L20 30L20 50L35 60L50 50L50 30z'></path>
            
            <path fill='rgb(161, 206, 255)' d='M35 20L20 30L35 40L50 30z'></path>
            <path fill='rgb(139, 190, 245)' d='M20 30L20 50L35 60L35 40z'></path>
            <path fill='rgb(123, 155, 189)' d='M50 30L50 50L35 60L35 40z'></path>
            
            </g>
            
            <g transform='scale(1,0.9) translate(37,14) rotate(45) scale(0.65,0.65)'>
            
            <path d='M 15 22L 15 28' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='5'></path>
            
            <path d='M 15 22L 15 27' fill='rgb(0,0,0,0)' stroke='rgb(171, 118, 19)' stroke-width='3'></path>
            
            <path d='M 15 7L 11 12L 13 12L 8 18L 12 18L 7 23L15 23' fill='rgb(0,160,0)' stroke='#57b5b1' stroke-width='1'></path>
            <path d='M 15 7L 11 12L 13 12L 8 18L 12 18L 7 23L15 23' fill='rgb(0,160,0)' stroke='#57b5b1' stroke-width='1' transform='translate(-15,0) scale(-1,1) translate(-44.75,0)'></path></g>
        
            <g transform='scale(0.9,1.2) translate(26,26) rotate(25) scale(0.5,0.5)'>
            <path d='M 13 24C 2 12 2 4 15 5M15 5C 29 5 22 18 12 24' fill='rgb(255,40,60)' stroke='#57b5b1' stroke-width='2'></path>
            
            <g transform='rotate(62) scale(0.7,1) translate(1,-14)'>
            <path d='M 6.8 4C 13 8 16 5 19 1M 15 -2C 13 8 13 5 14 10' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='4'></path>
            <path d='M 8 5C 13 8 16 5 18 2M 15 -1C 13 8 13 5 14 9' fill='rgb(0,0,0,0)' stroke='rgb(0,170,0)' stroke-width='2'></path>
            </g>
            
            <circle cx='10' cy='12' r='1' fill='rgb(255,255,40)'></circle><circle cx='18' cy='10' r='1' fill='rgb(255,255,40)'></circle><circle cx='13' cy='19' r='1' fill='rgb(255,255,40)'></circle><circle cx='15' cy='14' r='1' fill='rgb(255,255,40)'></circle>
            </g>
            
            <g transform='translate(38,32) rotate(9) scale(0.5,0.7) scale(0.75,0.75)'>
            <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15)'></path>
            <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(60)'></path>
            <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(120)'></path>
            <circle cx='15' cy='15' r='5' fill='rgb(255,255,0)'></circle>
            </g>
        </g>
        
    </svg>
    
    <svg id='loadedDice' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Loaded Dice</text>
        <text x='133' y='36' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Boosts 3 fields with x4</text>
        <text x='133' y='50' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>pollen for 15 mins. Baised</text>
        <text x='133' y='64' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>to the field you're in.</text>
        <text id='loadedDice_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(14,-3) rotate(-7) scale(0.8,0.8)'>
            <g transform='translate(0,-4) scale(1,1)'>
            
            <path fill='rgb(250,250,250)' stroke='black' stroke-width='3' d='M35 20L20 30L20 50L35 60L50 50L50 30z'></path>
            
            <path fill='rgb(250,250,250)' d='M35 20L20 30L35 40L50 30z'></path>
            <path fill='rgb(225,225,225)' d='M20 30L20 50L35 60L35 40z'></path>
            <path fill='rgb(200,200,200)' d='M50 30L50 50L35 60L35 40z'></path>
            
            </g>
            
            <g transform='scale(1,0.9) translate(37,14) rotate(45) scale(0.65,0.65)'>
            
            <path d='M 15 22L 15 28' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='5'></path>
            
            <path d='M 15 22L 15 27' fill='rgb(0,0,0,0)' stroke='rgb(171, 118, 19)' stroke-width='3'></path>
            
            <path d='M 15 7L 11 12L 13 12L 8 18L 12 18L 7 23L15 23' fill='rgb(0,160,0)' stroke='#57b5b1' stroke-width='1'></path>
            <path d='M 15 7L 11 12L 13 12L 8 18L 12 18L 7 23L15 23' fill='rgb(0,160,0)' stroke='#57b5b1' stroke-width='1' transform='translate(-15,0) scale(-1,1) translate(-44.75,0)'></path></g>
        
            <g transform='scale(0.9,1.2) translate(26,26) rotate(25) scale(0.5,0.5)'>
            <path d='M 13 24C 2 12 2 4 15 5M15 5C 29 5 22 18 12 24' fill='rgb(255,40,60)' stroke='#57b5b1' stroke-width='2'></path>
            
            <g transform='rotate(62) scale(0.7,1) translate(1,-14)'>
            <path d='M 6.8 4C 13 8 16 5 19 1M 15 -2C 13 8 13 5 14 10' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='4'></path>
            <path d='M 8 5C 13 8 16 5 18 2M 15 -1C 13 8 13 5 14 9' fill='rgb(0,0,0,0)' stroke='rgb(0,170,0)' stroke-width='2'></path>
            </g>
            
            <circle cx='10' cy='12' r='1' fill='rgb(255,255,40)'></circle><circle cx='18' cy='10' r='1' fill='rgb(255,255,40)'></circle><circle cx='13' cy='19' r='1' fill='rgb(255,255,40)'></circle><circle cx='15' cy='14' r='1' fill='rgb(255,255,40)'></circle>
            </g>
            
            <g transform='translate(38,32) rotate(9) scale(0.5,0.7) scale(0.75,0.75)'>
            <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15)'></path>
            <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(60)'></path>
            <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(120)'></path>
            <circle cx='15' cy='15' r='5' fill='rgb(255,255,0)'></circle>
            </g>
        </g>
        
        <g transform='translate(-2,-3) rotate(7) scale(0.8,0.8)'>
            <g transform='translate(0,-4) scale(1,1)'>
            
            <path fill='rgb(250,250,250)' stroke='black' stroke-width='3' d='M35 20L20 30L20 50L35 60L50 50L50 30z'></path>
            
            <path fill='rgb(150,150,150)' d='M35 20L20 30L35 40L50 30z'></path>
            <path fill='rgb(125,125,125)' d='M20 30L20 50L35 60L35 40z'></path>
            <path fill='rgb(80,80,80)' d='M50 30L50 50L35 60L35 40z'></path>
            
            </g>
            
            <g transform='scale(1,0.9) translate(37,14) rotate(45) scale(0.65,0.65)'>
            
            <path d='M 15 22L 15 28' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='5'></path>
            
            <path d='M 15 22L 15 27' fill='rgb(0,0,0,0)' stroke='rgb(171, 118, 19)' stroke-width='3'></path>
            
            <path d='M 15 7L 11 12L 13 12L 8 18L 12 18L 7 23L15 23' fill='rgb(0,160,0)' stroke='#57b5b1' stroke-width='1'></path>
            <path d='M 15 7L 11 12L 13 12L 8 18L 12 18L 7 23L15 23' fill='rgb(0,160,0)' stroke='#57b5b1' stroke-width='1' transform='translate(-15,0) scale(-1,1) translate(-44.75,0)'></path></g>
        
            <g transform='scale(0.9,1.2) translate(26,26) rotate(25) scale(0.5,0.5)'>
            <path d='M 13 24C 2 12 2 4 15 5M15 5C 29 5 22 18 12 24' fill='rgb(255,40,60)' stroke='#57b5b1' stroke-width='2'></path>
            
            <g transform='rotate(62) scale(0.7,1) translate(1,-14)'>
            <path d='M 6.8 4C 13 8 16 5 19 1M 15 -2C 13 8 13 5 14 10' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='4'></path>
            <path d='M 8 5C 13 8 16 5 18 2M 15 -1C 13 8 13 5 14 9' fill='rgb(0,0,0,0)' stroke='rgb(0,170,0)' stroke-width='2'></path>
            </g>
            
            <circle cx='10' cy='12' r='1' fill='rgb(255,255,40)'></circle><circle cx='18' cy='10' r='1' fill='rgb(255,255,40)'></circle><circle cx='13' cy='19' r='1' fill='rgb(255,255,40)'></circle><circle cx='15' cy='14' r='1' fill='rgb(255,255,40)'></circle>
            </g>
            
            <g transform='translate(38,32) rotate(9) scale(0.5,0.7) scale(0.75,0.75)'>
            <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15)'></path>
            <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(60)'></path>
            <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(120)'></path>
            <circle cx='15' cy='15' r='5' fill='rgb(255,255,0)'></circle>
            </g>
        </g>
        
        <g transform='translate(4,17) rotate(-17) scale(0.875,0.875)'>
            <g transform='translate(0,-4)'>
            
            <path fill='rgb(50,50,50)' stroke='black' stroke-width='3' d='M35 20L20 30L20 50L35 60L50 50L50 30z'></path>
            
            <path fill='rgb(50,50,50)' d='M35 20L20 30L35 40L50 30z'></path>
            <path fill='rgb(30,30,30)' d='M20 30L20 50L35 60L35 40z'></path>
            <path fill='rgb(3,3,3)' d='M50 30L50 50L35 60L35 40z'></path>
            
            </g>
            
            <g transform='scale(1,0.9) translate(37,14) rotate(45) scale(0.65,0.65)'>
            
            <path d='M 15 22L 15 28' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='5'></path>
            
            <path d='M 15 22L 15 27' fill='rgb(0,0,0,0)' stroke='rgb(171, 118, 19)' stroke-width='3'></path>
            
            <path d='M 15 7L 11 12L 13 12L 8 18L 12 18L 7 23L15 23' fill='rgb(0,160,0)' stroke='#57b5b1' stroke-width='1'></path>
            <path d='M 15 7L 11 12L 13 12L 8 18L 12 18L 7 23L15 23' fill='rgb(0,160,0)' stroke='#57b5b1' stroke-width='1' transform='translate(-15,0) scale(-1,1) translate(-44.75,0)'></path></g>
        
            <g transform='scale(0.9,1.2) translate(26,26) rotate(25) scale(0.5,0.5)'>
            <path d='M 13 24C 2 12 2 4 15 5M15 5C 29 5 22 18 12 24' fill='rgb(255,40,60)' stroke='#57b5b1' stroke-width='2'></path>
            
            <g transform='rotate(62) scale(0.7,1) translate(1,-14)'>
            <path d='M 6.8 4C 13 8 16 5 19 1M 15 -2C 13 8 13 5 14 10' fill='rgb(0,0,0,0)' stroke='#57b5b1' stroke-width='4'></path>
            <path d='M 8 5C 13 8 16 5 18 2M 15 -1C 13 8 13 5 14 9' fill='rgb(0,0,0,0)' stroke='rgb(0,170,0)' stroke-width='2'></path>
            </g>
            
            <circle cx='10' cy='12' r='1' fill='rgb(255,255,40)'></circle><circle cx='18' cy='10' r='1' fill='rgb(255,255,40)'></circle><circle cx='13' cy='19' r='1' fill='rgb(255,255,40)'></circle><circle cx='15' cy='14' r='1' fill='rgb(255,255,40)'></circle>
            </g>
            
            <g transform='translate(38,32) rotate(9) scale(0.5,0.7) scale(0.75,0.75)'>
            <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15)'></path>
            <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(60)'></path>
            <path d='M-2 0C -10 -18 10 -18 2 0M-2 0C -10 18 10 18 2 0' fill='rgb(0,80,255)' stroke='#57b5b1' stroke-width='2' transform='translate(15,15) rotate(120)'></path>
            <circle cx='15' cy='15' r='5' fill='rgb(255,255,0)'></circle>
            </g>
        </g>
        
    </svg>

    <svg id='jellyBeans' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='132' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Jelly Beans</text>
        <text x='133' y='37' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Scatters various buff-</text>
        <text x='133' y='50' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>granting beans on</text>
        <text x='133' y='63' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>nearby flowers.</text>
        <text id='jellyBeans_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(52,33) rotate(-14) scale(0.7,0.9)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(219, 203, 57)' d='M-15 -15 C -20 4 4 8 5 0C 9 -3 -8 -24 -15 -15'></path>
        <path stroke='rgb(255,255,255,0.6)' stroke-width='2.5' fill='rgb(0,0,0,0)' d='M-11 -7 C -15 -14 -5 -16 -8 -11M-5 0 C 1 1 4 0 0 -6'></path>
        <circle cx='-7' cy='-3' r='1.8' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-2' cy='0' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-10' cy='-8' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        
        </g>
        <g transform='translate(22,36) rotate(-7) scale(-1,1)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(252, 65, 65)' d='M-15 -15 C -20 4 4 8 5 0C 9 -3 -8 -24 -15 -15'></path>
        <path stroke='rgb(255,255,255,0.6)' stroke-width='2.5' fill='rgb(0,0,0,0)' d='M-11 -7 C -15 -14 -5 -16 -8 -11M-5 0 C 1 1 4 0 0 -6'></path>
        <circle cx='-7' cy='-3' r='1.8' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-2' cy='0' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-10' cy='-8' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        
        </g>
        <g transform='translate(44,42)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(80, 222, 250)' d='M-15 -15 C -20 4 4 8 5 0C 9 -3 -8 -24 -15 -15'></path>
        <path stroke='rgb(255,255,255,0.8)' stroke-width='3' fill='rgb(0,0,0,0)' d='M-11 -7 C -15 -14 -5 -16 -8 -11M-5 0 C 1 1 4 0 0 -6'></path>
        <circle cx='-7' cy='-3' r='1.8' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-2' cy='0' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-10' cy='-8' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        
        </g>
        
    </svg>

    <svg id='redExtract' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Red Extract</text>
        <text x='130' y='35' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Grants 1.25x  red</text>
        <text x='130' y='50' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>pollen for 10</text>
        <text x='130' y='65' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>minutes.</text>
        <text id='redExtract_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        <g transform='translate(0,4)'>
        <path d='M 47 17L 45 42L 37 47L 23 40L 29 16' fill='rgb(0,0,0,0.1)' stroke='rgb(0,0,0)' stroke-width='1.5'></path>
        <path d='M 45 34L 45 42L 37 47L 23 40L 26 30' fill='rgb(200,0,0)'></path>
        <rect x='31' y='2' width='19' height='10' fill='rgb(100,100,100' rx='2' transform='rotate(11)'></rect>
        <path d='M 48 12L 45 42L 37 47L 39 10' fill='rgb(0,0,0,0.5)'></path></g>
        
    </svg>
    
    <svg id='blueExtract' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Blue Extract</text>
        <text x='130' y='35' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Grants 1.25x  blue</text>
        <text x='130' y='50' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>pollen for 10</text>
        <text x='130' y='65' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>minutes.</text>
        <text id='blueExtract_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(0,4)'>
        <path d='M 47 17L 45 42L 37 47L 23 40L 29 16' fill='rgb(0,0,0,0.1)' stroke='rgb(0,0,0)' stroke-width='1.5'></path>
        <path d='M 45 34L 45 42L 37 47L 23 40L 26 30' fill='rgb(0,0,255)'></path>
        <rect x='31' y='2' width='19' height='10' fill='rgb(100,100,100' rx='2' transform='rotate(11)'></rect>
        <path d='M 48 12L 45 42L 37 47L 39 10' fill='rgb(0,0,0,0.5)'></path></g>
        
    </svg>
    
    <svg id='glitter' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Glitter</text>
        <text x='130' y='35' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Boosts the field you're</text>
        <text x='130' y='50' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>in, giving +100% pollen</text>
        <text x='130' y='65' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>pollen for 15 minutes.</text>
        <text id='glitter_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        <g transform='scale(0.9,0.9) translate(3,-3)'>
        <path fill='rgb(251, 237, 255)' d='M 35 39 C 68 14 77 80 33 48'></path>
        <path fill='rgb(204, 228, 255)' stroke='rgb(0,0,0)' stroke-width='1.5' d='M 29 21 C 55 23 41 39 50 50C 53 68 33 51 37 54C 33 44 8 37 29 21' transform='translate(-27,26) scale(1.1,1.175) rotate(-38)'></path>
        <path fill='rgb(251, 237, 255)' d='M 46 43 C 68 14 77 80 43 48'></path>
        <circle cx='50' cy='53' r='1' fill='rgb(247, 222, 111)'></circle><circle cx='57' cy='41' r='1' fill='rgb(247, 222, 111)'></circle><circle cx='60' cy='51' r='1' fill='rgb(247, 222, 111)'></circle><circle cx='48' cy='45' r='1' fill='rgb(247, 222, 111)'></circle><circle cx='56' cy='53' r='1' fill='rgb(225, 177, 242)'></circle><circle cx='56' cy='47' r='1' fill='rgb(225, 177, 242)'></circle><circle cx='62' cy='45' r='1' fill='rgb(225, 177, 242)'></circle><circle cx='48' cy='48' r='1' fill='rgb(225, 177, 242)'></circle>
        <path stroke='rgb(181, 139, 49)' fill='rgb(0,0,0,0)' stroke-width='5' d='M 25 55 C 46 46 28 38 45 33'></path>
        <path stroke='rgb(250, 239, 142)' fill='rgb(0,0,0,0)' stroke-width='5' d='M 20 40 C 14 26 24 25 26 23'></path>
        </g>
        
    </svg>
    
    <svg id='glue' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Glue</text>
        <text x='130' y='35' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Grants 1.25x pollen</text>
        <text x='130' y='50' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>from bees and tools</text>
        <text x='130' y='65' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>for 10 minutes.</text>
        <text id='glue_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        <rect x='33' y='16' rx='6' width='20' height='30' fill='rgb(255,255,255)' stroke='rgb(0,0,0)' stroke-width='1.5' transform='translate(0,-5) rotate(15)'></rect>
        <rect x='39.5' y='6' rx='1' width='8' height='10' fill='rgb(255,150,0)' stroke='rgb(0,0,0)' stroke-width='1.5' transform='translate(0,-5) rotate(15)'></rect>
        <path fill='rgb(255,30,255)' d='M10 10L20 10L10 25Z' transform='translate(0,-5) rotate(15) translate(28,12)'></path>
        <path fill='rgb(25,255,100)' d='M10 10L20 10L10 25Z' transform='translate(0,-5) translate(44,62) rotate(195)'></path>
        
    </svg>
    
    <svg id='oil' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Oil</text>
        <text x='130' y='35' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Grants extra bee</text>
        <text x='130' y='50' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>and player speed</text>
        <text x='130' y='65' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>for 10 minutes.</text>
        <text id='oil_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        <path fill='rgb(255,255,120)' stroke='rgb(0,0,0)' stroke-width='1.5' d='M35 55C17 54 17 36 28 30C 31 28 31 21 30 20' transform='translate(0,0)'></path>
        <path fill='rgb(255,255,120)' stroke='rgb(0,0,0)' stroke-width='1.5' d='M35 55C17 54 17 36 28 30C 31 28 31 21 30 20' transform='scale(-1,1) translate(-70,0)'></path>
        <path fill='rgb(255,255,120)' d='M35 55L31 27L40 27Z' transform='translate(0,0)'></path>
        <rect x='30.5' y='11' width='9' height='10' stroke='rgb(0,0,0)' stroke-width='1.5' rx='3' fill='rgb(200,150,0)' transform='translate(0,0)'></rect>
        <rect x='21' y='38' width='28' height='6'rx='3' fill='rgb(150,70,0)' transform='translate(0,0)'></rect>
        <circle cx='35' cy='40' r='5' fill='rgb(255,255,255)'></circle>
        
    </svg>
    
    <svg id='enzymes' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Enzymes</text>
        <text x='130' y='35' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Grants x1.5 convert</text>
        <text x='130' y='50' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>rate and 10% instant</text>
        <text x='130' y='62' style='font-family:trebuchet ms;font-size:10px;' fill='rgb(0,0,0)' text-anchor='middle'>conversion for 10 minutes.</text>
        <text id='enzymes_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <path d='M20 45 C20 30 50 30 50 45M20 45L20 47C20 60 50 60 50 47 L50 45' fill='rgb(100,255,100)' stroke='rgb(0,0,0)' strokeWeight='1.5' transform='translate(36,-15) scale(0.9,0.9) rotate(25)'></path>
        
        <path d='M20 45 C20 30 50 30 50 45M20 45L20 47C20 60 50 60 50 47 L50 45' fill='rgb(255,255,80)' stroke='rgb(0,0,0)' strokeWeight='1.5' transform='translate(-11,-9)'></path>
        
        <path d='M20 45 C20 30 50 30 50 45M20 45L20 47C20 60 50 60 50 47 L50 45' fill='rgb(0,0,0,0.2)' transform='translate(0,11) scale(0.7,0.5)'></path>
        
        <path d='M20 45 C20 30 50 30 50 45M20 45L20 47C20 60 50 60 50 47 L50 45' fill='rgb(0,0,0,0.2)' transform='translate(52,5) scale(0.7,0.5) rotate(44)'></path>
        
    </svg>
    
    <svg id='tropicalDrink' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Tropical Drink</text>
        <text x='130' y='35' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Grants 1.25x white</text>
        <text x='130' y='50' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>pollen and 5% critical</text>
        <text x='130' y='65' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>chance for 10 minutes.</text>
        <text id='tropicalDrink_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        <path d='M15 30C15 60 60 60 60 30C 60 15 15 15 15 30' fill='rgb(163, 90, 18)' stroke='rgb(0,0,0)'stroke-width='1.5'></path>
        <path d='M20 30C20 42 55 42 55 30C 55 20 20 20 20 30' fill='rgb(255,255,210)' stroke='rgb(0,0,0)'stroke-width='1.5'></path>
        <path d='M 44 37L 39 38L 48 12L 60 7L 61 11L 52 15Z' fill='rgb(235, 112, 235)' stroke='rgb(0,0,0)'stroke-width='1.5'></path>
        
    </svg>
    
    <svg id='purplePotion' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Purple Potion</text>
        <text x='130' y='35' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Grants better stats</text>
        <text x='130' y='50' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>of glue and red/blue</text>
        <text x='130' y='65' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>extracts for 10 minutes.</text>
        <text id='purplePotion_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        <path d='M50 50L 20 50L 32 25L32 15L37 15L37 25Z' fill='rgb(0,0,0,0.3)' stroke='rgb(0,0,0)' strokeWeight='1.5' transform='rotate(15) translate(10,-8)'></path>
        <path d='M50 50L 20 50L 27 38L43 38Z' fill='rgb(200,0,200)' transform='rotate(15) translate(10,-8)'></path>
        <rect x='30.5' y='10' width='8' height='6' transform='rotate(15) translate(10,-8)'></rect>
        
    </svg>
    
    <svg id='superSmoothie' style='width:200px;height:70px;cursor:pointer;border-radius:5px;'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Super Smoothie</text>
        <text x='130' y='35' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Grants improved buffs</text>
        <text x='130' y='50' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>of all items for 10 </text>
        <text x='130' y='65' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>minutes.</text>
        <text id='superSmoothie_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        <path d='M 20 45C20 55 50 55 50 45L 50 35 L20 35' fill='rgb(50,255,50)' transform='translate(0,3)'></path>
        <path d='M 50 43C 56 40 57 41 59 35' stroke-width='5' fill='rgb(0,0,0,0)' stroke='rgb(50,255,50)' transform='translate(0,3)'></path>
        <path d='M20 45C20 55 50 55 50 45C65 45 65 20 50 20C50 10 20 10 20 20Z' fill='rgb(0,0,0,0)' stroke='rgb(0,0,0)' stroke-width='1.5' transform='translate(0,3)'></path>
        <path d='M50 40 C 60 40 60 25 50 25M50 40.6 L 50 24.4' fill='rgb(225,225,225,0)' stroke='rgb(0,0,0)' stroke-width='1.5' transform='translate(0,3)'></path>
        <rect transform='translate(0,3) rotate(-17)' x='19' y='11' width='5' height='30' stroke='rgb(0,0,0)' stroke-width='1.5' fill='rgb(255,80,80)'></rect>
        <path d='M50 20C50 25 20 25 20 20' fill='rgb(0,0,0,0)' stroke='rgb(0,0,0)' stroke-width='1.5' transform='translate(0,3)'></path>
        <circle cx='35' cy='37.4' r='6' fill='rgb(255,0,50)'></circle>
        <path d='M36 32C 30 30 30 35 35 35C 37 35 37 39 32 38' stroke='rgb(255,255,255)' stroke-width='2' fill='rgb(0,0,0,0)' transform='translate(0,3)'></path>
        
    </svg>
    
    <svg id='magicBean' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='132' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Magic Bean</text>
        <text x='133' y='39' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Plants a random</text>
        <text x='133' y='53' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>sprout in the field</text>
        <text x='133' y='67' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>you're in.</text>
        <text id='magicBean_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(29,46) scale(-1.2,1.1) rotate(-8)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(40, 212, 71)' d='M-16 -11 C -15 -24 -18 -26 -14 -28C -5 -34 -1 -29 3 -21L -4 -24L 2 -19C -5 -15 -11 -19 -12 -21L -11 -10' transform='translate(4,-1) scale(1.2,1)'></path>     
        <path stroke='black' stroke-width='1.5' fill='rgb(40, 212, 71)' d='M-15 -15 C -20 4 4 8 5 0C 8 -3 -1 -24 -15 -15'></path>
        <path stroke='rgb(255,255,255,0.8)' stroke-width='2.5' fill='rgb(0,0,0,0)' d='M-11 -7 C -15 -14 -5 -16 -8 -11M-5 0 C 1 1 4 0 0 -6'></path>
        <circle cx='-7' cy='-3' r='1.8' fill='rgb(255,255,255,0.5)'></circle>
        <circle cx='-2' cy='0' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-10' cy='-8' r='1.5' fill='rgb(0,0,0,0.2)'></circle>
        <circle cx='-8' cy='-27' r='1.5' fill='rgb(255,255,255,0.8)'></circle>
        <circle cx='-3' cy='-21' r='1.5' fill='rgb(255,255,255,0.8)'></circle>
        <circle cx='-13' cy='-21' r='1.5' fill='rgb(255,255,255,0.8)'></circle>
        
        </g>
        
    </svg>
    
    <svg id='cloudVial' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='132' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Cloud Vial</text>
        <text x='133' y='39' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Summons a cloud</text>
        <text x='133' y='53' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>on the field you're in,</text>
        <text x='133' y='67' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>lasting for 3 mins.</text>
        <text id='cloudVial_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(36,33) rotate(-7)'>
        <path stroke='black' stroke-width='1.5' fill='rgb(0,0,0,0.25)' d='M-10 -15L10 -15C10 -1 15 -14 15 15C15 25 -15 25 -15 15C-15 -15 -10 -1 -10 -15'></path>
        <rect stroke='black' stroke-width='1.5' fill='rgb(181, 120, 60)' x='-11' y='-20' width='22' height='6' rx='2'></rect>
        
        <g transform='translate(3,6) scale(1.1,1.1)'>
        <circle fill='rgb(255,255,255)' cx='0' cy='0' r='5'></circle>
        <circle fill='rgb(0,0,0,0.2)' cx='0.3' cy='1.1' r='4'></circle>
        <circle fill='rgb(255,255,255)' cx='-0.4' cy='-1.0' r='3'></circle></g>
        <g transform='translate(-4,9) scale(1.2,1.2)'>
        <circle fill='rgb(255,255,255)' cx='0' cy='0' r='5'></circle>
        <circle fill='rgb(0,0,0,0.2)' cx='0.3' cy='1.1' r='4'></circle>
        <circle fill='rgb(255,255,255)' cx='-0.4' cy='-1.0' r='3'></circle></g>
        <g transform='translate(4,11) scale(1.1,1.1)'>
        <circle fill='rgb(255,255,255)' cx='0' cy='0' r='5'></circle>
        <circle fill='rgb(0,0,0,0.2)' cx='0.3' cy='1.1' r='4'></circle>
        <circle fill='rgb(255,255,255)' cx='-0.4' cy='-1.0' r='3'></circle></g>
        <g transform='translate(0,1) scale(1,1)'>
        <circle fill='rgb(255,255,255)' cx='0' cy='0' r='5'></circle>
        <circle fill='rgb(0,0,0,0.2)' cx='0.3' cy='1.1' r='4'></circle>
        <circle fill='rgb(255,255,255)' cx='-0.4' cy='-1.0' r='3'></circle></g>
        <g transform='translate(-4,4) scale(1,1)'>
        <circle fill='rgb(255,255,255)' cx='0' cy='0' r='5'></circle>
        <circle fill='rgb(0,0,0,0.2)' cx='0.3' cy='1.1' r='4'></circle>
        <circle fill='rgb(255,255,255)' cx='-0.4' cy='-1.0' r='3'></circle></g>
        
        <path stroke='rgb(58, 127, 207)' stroke-width='2.5' fill='rgb(0,0,0,0)' d='M-12 -6C-12 -5 12 -5 12 -6M6 -6C 16 2 1 0 11 7'></path>
        </g>
    </svg>

    <svg id='antPass' style='width:200px;height:70px;cursor:pointer;border-radius:5px;'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='134' y='19' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Ant Pass</text>
        <text x='132' y='39' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Grants you 1 addmission</text>
        <text x='132' y='52' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>to the Ant Challenge!</text>
        <text id='antPass_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(35,35)'>
        <path fill='rgb(255, 255, 160)' stroke='black' stroke-width='2' d='M -20 -15L 15 -15L 20 -10L 20 10L 15 15L-20 15L-20 -15'></path>
        <path fill='rgb(0,0,0,0)' stroke='rgb(0,170,0)' stroke-width='4' d='M -17 -14L -17 14M 13 -14L 18 -8L 18 8L 13 14M -8 -4L7 -4L4 -8L-4 -8L-8 -2'></path>
        <path fill='rgb(50,50,50)' d='M -8 -4L8 -4L8 8L-8 8'></path>
        <path stroke='rgb(50,50,50)' fill='rgb(0,0,0,0)' stroke-width='2' d='M -3 -5L-7 -10M 3 -5L7 -10'></path>
        <circle cx='-4' cy='0' r='2' fill='rgb(255,40,40)'></circle>
        <circle cx='4' cy='0' r='2' fill='rgb(255,40,40)'></circle>
        
        </g>
    </svg>
    
    <svg id='roboPass' style='width:200px;height:70px;cursor:pointer;border-radius:5px;'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='134' y='19' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Robo Pass</text>
        <text x='132' y='39' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Grants you 1 addmission</text>
        <text x='132' y='52' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>to the Robo Challenge!</text>
        <text id='roboPass_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(35,35)'>
        <path fill='rgb(255, 255, 160)' stroke='black' stroke-width='2' d='M -20 -15L 15 -15L 20 -10L 20 10L 15 15L-20 15L-20 -15'></path>
        <path fill='rgb(0,0,0,0)' stroke='rgb(130,160,130)' stroke-width='4' d='M -17 -14L -17 14M 13 -14L 18 -8L 18 8L 13 14'></path>
        <circle cx='0' cy='0' r='6' fill='rgb(100,150,100)'></circle>
        <circle cx='0' cy='0' r='3' fill='rgb(255, 255, 160)'></circle>
        
        <rect x='-2.5' y='-9' fill='rgb(100,150,100)' width='5' height='4' transform='rotate(0)'></rect>
        <rect x='-2.5' y='-9' fill='rgb(100,150,100)' width='5' height='4' transform='rotate(45)'></rect>
        <rect x='-2.5' y='-9' fill='rgb(100,150,100)' width='5' height='4' transform='rotate(90)'></rect>
        <rect x='-2.5' y='-9' fill='rgb(100,150,100)' width='5' height='4' transform='rotate(135)'></rect>
        <rect x='-2.5' y='-9' fill='rgb(100,150,100)' width='5' height='4' transform='rotate(180)'></rect>
        <rect x='-2.5' y='-9' fill='rgb(100,150,100)' width='5' height='4' transform='rotate(220)'></rect>
        <rect x='-2.5' y='-9' fill='rgb(100,150,100)' width='5' height='4' transform='rotate(265)'></rect>
        <rect x='-2.5' y='-9' fill='rgb(100,150,100)' width='5' height='4' transform='rotate(310)'></rect>
        
        </g>
    </svg>
    
    <svg id='cog' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='132' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Cog</text>
        <text x='133' y='37' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Used in the Robo</text>
        <text x='133' y='50' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Challenge. Removed</text>
        <text x='134' y='63' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>when the challenge ends.</text>
        <text id='cog_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(35,35) scale(1.2,1.2)'>
        
        <rect x='-4' y='-14' width='8' height='28' fill='rgb(20,195,0)' stroke='black' stroke-width='1.5'></rect>
        <rect x='-4' y='-14' width='8' height='28' fill='rgb(20,195,0)' stroke='black' stroke-width='1.5' transform='rotate(60)'></rect>
        <rect x='-4' y='-14' width='8' height='28' fill='rgb(20,195,0)' stroke='black' stroke-width='1.5' transform='rotate(120)'></rect>
        <circle cx='0' cy='0' r='6' fill='rgb(0,0,0,0)' stroke-width='5' stroke='rgb(20,195,0)' ></circle>
        <circle cx='0' cy='0' r='5' fill='rgb(50,50,50)'></circle>
        
        </g>
    </svg>

    <svg id='translator' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Translator</text>
        <text x='130' y='35' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Allows you to talk to</text>
        <text x='130' y='49' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Gifted Bucko and</text>
        <text x='130' y='63' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Riley Bee or Stickbug.</text>
        <text id='translator_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='scale(-1,1) rotate(-45) scale(1,1.3) rotate(58) scale(1,1) translate(-28,37) scale(0.9,0.9)'>

        <path fill='rgb(150,150,150)' stroke='black' stroke-width='2' d='M -5 0L0 -5L5 0L0 5z' transform='scale(1,1) translate(-10,-12)'></path>
        
        <path fill='rgb(150,150,150)' stroke='black' stroke-width='2' d='M -20 0L0 -15L20 0L0 15z'></path>
        <path fill='rgb(115,115,115)' stroke='black' stroke-width='2' d='M0 25L-20 10L-20 0L0 15z'></path>
        <path fill='rgb(100,100,100)' stroke='black' stroke-width='2' d='M0 25L20 10L20 0L0 15z'></path>
        
        <path fill='rgb(230,230,230)' stroke='black' stroke-width='2.5' d='M -20 0L0 -15L20 0L0 15z' transform='scale(0.35,0.35) translate(-3,-15)'></path>
        <path fill='rgb(230,230,230)' stroke='black' stroke-width='2.5' d='M -20 0L0 -15L20 0L0 15z' transform='scale(0.35,0.35) translate(-21,-1)'></path>
        
        <path fill='rgb(225,195,0)' stroke='black' stroke-width='3' d='M -20 0L0 -15L20 0L0 15z' transform='scale(0.2,0.2) translate(42,1)'></path>
        <path fill='rgb(230,60,50)' stroke='black' stroke-width='3' d='M -20 0L0 -15L20 0L0 15z' transform='scale(0.2,0.2) translate(6,28)'></path>
        
        </g>
    </svg>

    <svg id='spiritPetal' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Spirit Petal</text>
        <text x='130' y='35' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>A delicate flower</text>
        <text x='130' y='49' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>petal used for crafting</text>
        <text x='130' y='63' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>powerful rare items.</text>
        <text id='spiritPetal_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='scale(1.1,1.1) translate(-5,3)'>
        <path fill='rgb(219, 206, 222)' stroke='black' stroke-width='1.7' d='M 20 39C 30 42 44 48 57 27C 41 9 25 15 20 39'></path>
        <path fill='rgb(0,0,0,0.1)' d='M 20 39C 30 42 44 48 57 27C 41 9 25 15 20 39' transform='scale(0.6,0.6) translate(26,19)'></path>
        <path fill='rgb(0,0,0,0.08)' d='M 20 39C 30 42 44 48 57 27C 41 9 25 15 20 39' transform='scale(0.7,0.5) translate(20,36)'></path></g>
        <path stroke='white' stroke-width='1.75' d='M-3 2L3 0M-1 -2L1 4' transform='translate(51,26) rotate(-12) scale(1.2,1.2)'></path>
        <path stroke='white' stroke-width='1.75' d='M-3 2L3 0M-1 -2L1 4' transform='translate(35,26) rotate(44) scale(1.1,1.1)'></path>
        <path stroke='white' stroke-width='1.75' d='M-3 2L3 0M-1 -2L1 4' transform='translate(42,41) rotate(23) scale(1.1,1.1)'></path>
        <path stroke='white' stroke-width='1.75' d='M-3 2L3 0M-1 -2L1 4' transform='translate(23,35) rotate(20) scale(1.3,1.3)'></path>
        
    </svg>
    
    <svg id='treat' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Treat</text>
        <text x='130' y='37' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Improves a bee's </text>
        <text x='130' y='53' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>bond by 10.</text>
        <text id='treat_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <path d='M 26 40C 14 39 4 12 30 28C 35 1 45 12 50 26C 68 10 73 31 60 41C 67 58 51 74 41 52C 37 58 2 74 26 40' stroke='rgb(0,0,0)' fill='rgb(138, 88, 18)' stroke-width='2' transform='scale(0.9,0.9) translate(-5,0)'></path>
        
        <path d='M 27 27L 29 34M 37 27L 39 34M 30 39 L 35 42L40 37' stroke='rgb(255,255,255)' fill='rgb(0,0,0,0)' stroke-width='2'></path>
        
    </svg>

    <svg id='starTreat' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Star Treat</text>
        <text x='130' y='35' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Improves a bee's </text>
        <text x='130' y='49' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>bond by 1,000 and</text>
        <text x='130' y='64' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>makes it gifted!</text>
        <text id='starTreat_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <path d='M 26 40C 14 39 4 12 30 28C 35 1 45 12 50 26C 68 10 73 31 60 41C 67 58 51 74 41 52C 37 58 2 74 26 40' stroke='rgb(0,0,0)' fill='rgb(186, 157, 54)' stroke-width='2' transform='scale(0.9,0.9) translate(-2,0)'></path>
        
        <path fill='rgb(255, 255, 110)' d='M5.71,16.85L8.23,26.33L0.00,21.00L-8.23,26.33L-5.71,16.85L-13.31,10.67L-3.53,10.15L-0.00,1.00L3.53,10.15L13.31,10.67L5.71,16.85z' transform='translate(34,23) scale(0.75,0.75) rotate(-7)' stroke='black' stroke-width='1.5'></path>
        
        <path d='M 31 14L 32 21M 52 23L 46 28M 44 52L 42 45M 16 50L 22 43M 13 25L 19 29' stroke='rgb(240, 255, 145)' fill='rgb(0,0,0,0)' stroke-width='2' transform='translate(2,0)'></path>
        
    </svg>
    
    <svg id='atomicTreat' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Atomic Treat</text>
        <text x='130' y='35' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Improves a bee's </text>
        <text x='130' y='49' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>bond by 1,000 and</text>
        <text x='130' y='63' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>causes a mutation.</text>
        <text id='atomicTreat_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <path d='M 26 40C 14 39 4 12 30 28C 35 1 45 12 50 26C 68 10 73 31 60 41C 67 58 51 74 41 52C 37 58 2 74 26 40' stroke='rgb(0,0,0)' fill='rgb(118, 191, 105)' stroke-width='2' transform='scale(0.9,0.9) translate(-1,0)'></path>
        
        <g transform='translate(-17,6) scale(1.3,1.3) rotate(-18) scale(0.9,1)'>
        <circle cx='0' cy='0' fill='rgb(0,0,0,0)' stroke='rgb(173, 56, 56)' stroke-width='2' r='8' transform='translate(37,34) scale(0.4,1)'></circle>
        <circle cx='0' cy='0' fill='rgb(0,0,0,0)' stroke='rgb(173, 56, 56)' stroke-width='2' r='8' transform='translate(37,34) rotate(60) scale(0.4,1)'></circle>
        <circle cx='0' cy='0' fill='rgb(0,0,0,0)' stroke='rgb(173, 56, 56)' stroke-width='2' r='8' transform='translate(37,34) rotate(120) scale(0.4,1)'></circle>
        </g>
        
        <circle cx='37' cy='30' fill='rgb(230,230,230)' r='2'></circle>
        <circle cx='45' cy='40' fill='rgb(230,230,230)' r='2'></circle>
        <circle cx='30' cy='39' fill='rgb(230,230,230)' r='2'></circle>
        
    </svg>
    
    <svg id='blueberry' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Blueberry</text>
        <text x='130' y='39' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Improves your bee's</text>
        <text x='130' y='54' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>bond by 25.</text>
        <text id='blueberry_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <circle cx='16' cy='47' r='17' stroke='rgb(0,0,0)' strokeWeight='1.5' fill='rgb(88, 88, 219)' transform='scale(1,0.9) rotate(-26)'></circle>
        <circle cx='16' cy='47' r='7' fill='rgb(32, 32, 150)' transform='scale(1,0.7) rotate(-26)'></circle>
        
    </svg>
    
    <svg id='strawberry' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Strawberry</text>
        <text x='130' y='39' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Improves your bee's</text>
        <text x='130' y='54' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>bond by 25.</text>
        <text id='strawberry_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        <path stroke='rgb(0,0,0)' stroke-width='1.5' fill='rgb(255,0,25)' d='M 24 27 L 46 22C 83 11 41 71 42 61C 20 39 15 36 24 27' transform='translate(7,3) scale(0.7,0.8)'></path>
        <path stroke='rgb(0,200,0)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 38 24 C 37 16 34 16 24 11M 38 24 C 43 16 34 16 38 6M 38 24 C 37 16 21 16 24 21M 38 24 C 46 16 46 16 45 8' transform='translate(7,3) scale(0.7,0.8)'></path>
        <circle cx='35' cy='35' fill='rgb(250,170,100)' r='1'></circle><circle cx='27' cy='29' fill='rgb(250,170,100)' r='1'></circle><circle cx='35' cy='25' fill='rgb(250,170,100)' r='1'></circle><circle cx='40' cy='42' fill='rgb(250,170,100)' r='1'></circle><circle cx='43' cy='36' fill='rgb(250,170,100)' r='1'></circle><circle cx='41' cy='28' fill='rgb(250,170,100)' r='1'></circle><circle cx='31' cy='40' fill='rgb(250,170,100)' r='1'></circle>
        
    </svg>
    
    <svg id='pineapple' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Pineapple</text>
        <text x='130' y='39' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Improves your bee's</text>
        <text x='130' y='54' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>bond by 25.</text>
        <text id='pineapple_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        <path transform='scale(1.1,1.1) translate(-10,-5)'stroke='rgb(0,0,0)' stroke-width='1.5' fill='rgb(255,255,25)' d='M37 20L50 25L60 45C 56 52 43 53 34 49L 22 43Z' ></path><path fill='rgb(0,0,0,0.1)' d='M50 25L60 45C 56 52 43 53 34 49Z' transform='scale(1.1,1.1) translate(-10,-5)'></path><path stroke='white' stroke-width='3' d='M38 22L30 35'transform='scale(1.1,1.1) translate(-10,-5)'></path><path stroke='rgb(0,0,0,0.3)' stroke-width='2' d='M35 35 L 38 30M 45 52 L 46 45M 51 51 L 50 45M 58 48 L 54 44' transform='scale(1.1,1.1) translate(-10,-5)'></path>
        
    </svg>
    
    <svg id='sunflowerSeed' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:16px;' fill='rgb(0,0,0)' text-anchor='middle'>Sunflower Seed</text>
        <text x='130' y='39' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Improves your bee's</text>
        <text x='130' y='54' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>bond by 25.</text>
        <text id='sunflowerSeed_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        <path stroke='rgb(0,0,0)' stroke-width='1.5' fill='rgb(252,231,202)' d='M37 20C 8 36 11 46 20 50C 44 63 43 31 37 20' transform='translate(5,0)'></path>
        
        <path stroke='rgb(98,99,90)' stroke-width='3' fill='rgb(0,0,0,0)' d='M37 20C 25 36 20 46 20 50M37 20C 14 36 17 46 20 50M37 20C 38 36 36 58 20 50M37 20C 36 36 20 49 20 50' transform='translate(5,0)'></path>
        
    </svg>
	
    <svg id='bitterberry' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Bitterberry</text>
        <text x='130' y='37' style='font-family:trebuchet ms;font-size:11.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Raises the bond of a</text>
        <text x='130' y='51' style='font-family:trebuchet ms;font-size:11.5px;' fill='rgb(0,0,0)' text-anchor='middle'> bee by 100 and  has a</text>
        <text x='130' y='64' style='font-family:trebuchet ms;font-size:11.5px;' fill='rgb(0,0,0)' text-anchor='middle'> chance to mutate it.</text>
        <text id='bitterberry_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='scale(0.85,0.85) translate(6,7)'>
        
        <path fill='rgb(204, 189, 88)' stroke='black' stroke-width='2' d='M35 16C 66 23 45 50 24 50C 12 57 19 55 18 48C 9 15 27 15 35 16'></path>
        
        <path fill='rgb(129, 57, 82)' stroke='black' stroke-width='2' d='M 35 12C 64 13 49 27 36 16C -2 11 38 3 36 12' transform='translate(4,1)'></path>
        
        <path fill='rgb(138, 78, 18)' stroke='black' stroke-width='2' d='M 37 13C 39 12 43 7 48 5C 50 12 48 7 43 13' transform='translate(2,0)'></path>
        
        <circle cx='39' cy='37' r='1.5' fill='rgb(138, 89, 15,0.5)'></circle><circle cx='41' cy='28' r='1.5' fill='rgb(138, 89, 15,0.5)'></circle><circle cx='31' cy='41' r='1.5' fill='rgb(138, 89, 15,0.5)'></circle><circle cx='25' cy='43' r='1.5' fill='rgb(171, 119, 36,0.5)'></circle><circle cx='46' cy='33' r='1.5' fill='rgb(138, 89, 15,0.5)'></circle>
        
        </g>
        
    </svg>
    
    <svg id='neonberry' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Neonberry</text>
        <text x='130' y='37' style='font-family:trebuchet ms;font-size:11.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Raises the bond of a</text>
        <text x='130' y='51' style='font-family:trebuchet ms;font-size:11.5px;' fill='rgb(0,0,0)' text-anchor='middle'> bee by 500 and </text>
        <text x='130' y='64' style='font-family:trebuchet ms;font-size:11.5px;' fill='rgb(0,0,0)' text-anchor='middle'> makes it radioactive.</text>
        <text id='neonberry_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='scale(0.825,0.825) translate(8,8)'>
        
        <circle fill='rgb(88, 227, 88)' cx='35' cy='35' stroke='black' stroke-width='2' r='22'></circle>
        
        <circle fill='rgb(255,255,255,0.4)' cx='35' cy='35' r='10'></circle><circle fill='rgb(255,255,255,0.4)' cx='35' cy='35' r='4'></circle>
        
        <path fill='rgb(0,0,0,0)' stroke='black' stroke-width='1' d='M 24 30C 28 12 41 22 40 20M 24 41C 24 49 37 50 36 48M 42 30C 47 28 46 43 39 43'></path>
        
        <path fill='rgb(138, 78, 18)' stroke='black' stroke-width='2' d='M 37 13C 39 12 43 7 42 5C 50 12 48 7 41 15' transform='translate(6,1)'></path>
        
        </g>
        
    </svg>

    <svg id='moonCharm' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Moon Charm</text>
        <text x='134' y='37' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Raises the bond of a</text>
        <text x='134' y='51' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'> bee by 250. Also used </text>
        <text x='134' y='64' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>to make special items.</text>
        <text id='moonCharm_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='scale(1.2,1.2) translate(14,16) rotate(-24)'>
        <path stroke='black' stroke-width='1.5' fill='rgb(237, 255, 102)' d='M 22 26C -7 39 -4 -3 19 8C 13 5 5 25 22 26' ></path>
        
        </g>
        
    </svg>
    
    <svg id='redDrive' style='width:200px;height:70px;cursor:pointer;border-radius:5px;'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='134' y='19' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Red Drive</text>
        <text x='135' y='38' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Gives red pollen, capacity,</text>
        <text x='134' y='52' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'> and bee attack for 3 mins</text>
        <text x='132' y='65' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>in the Robo Challenge.</text>
        <text id='redDrive_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(35,29)'>
        <path fill='rgb(100,100,100)' stroke='black' stroke-width='2' d='M -20 0L0 -15L20 0L0 15z'></path>
        <path fill='rgb(85,85,85)' stroke='black' stroke-width='2' d='M0 25L-20 10L-20 0L0 15z'></path>
        <path fill='rgb(70,70,70)' stroke='black' stroke-width='2' d='M0 25L20 10L20 0L0 15z'></path>
        
        <circle cx='0' cy='0' r='8' stroke='rgb(155,0,0)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        <circle cx='0' cy='-5' r='8' stroke='rgb(255,0,0)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        
        <rect x='-19' y='8' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        <rect x='-13' y='11' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        <rect x='-7' y='14' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        
        </g>
    </svg>
    
    <svg id='whiteDrive' style='width:200px;height:70px;cursor:pointer;border-radius:5px;'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='134' y='19' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>White Drive</text>
        <text x='135' y='38' style='font-family:trebuchet ms;font-size:10.2px;' fill='rgb(0,0,0)' text-anchor='middle'>Gives white pollen, capacity,</text>
        <text x='134' y='52' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'> and bee attack for 3 mins</text>
        <text x='132' y='65' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>in the Robo Challenge.</text>
        <text id='whiteDrive_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(35,29)'>
        <path fill='rgb(100,100,100)' stroke='black' stroke-width='2' d='M -20 0L0 -15L20 0L0 15z'></path>
        <path fill='rgb(85,85,85)' stroke='black' stroke-width='2' d='M0 25L-20 10L-20 0L0 15z'></path>
        <path fill='rgb(70,70,70)' stroke='black' stroke-width='2' d='M0 25L20 10L20 0L0 15z'></path>
        
        <circle cx='0' cy='0' r='8' stroke='rgb(175,175,175)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        <circle cx='0' cy='-5' r='8' stroke='rgb(255,255,255)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        
        <rect x='-19' y='8' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        <rect x='-13' y='11' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        <rect x='-7' y='14' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        
        </g>
    </svg>
    
    <svg id='blueDrive' style='width:200px;height:70px;cursor:pointer;border-radius:5px;'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='134' y='19' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Blue Drive</text>
        <text x='135' y='38' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Gives blue pollen, capacity,</text>
        <text x='134' y='52' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'> and bee attack for 3 mins</text>
        <text x='132' y='65' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>in the Robo Challenge.</text>
        <text id='blueDrive_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(35,29)'>
        <path fill='rgb(100,100,100)' stroke='black' stroke-width='2' d='M -20 0L0 -15L20 0L0 15z'></path>
        <path fill='rgb(85,85,85)' stroke='black' stroke-width='2' d='M0 25L-20 10L-20 0L0 15z'></path>
        <path fill='rgb(70,70,70)' stroke='black' stroke-width='2' d='M0 25L20 10L20 0L0 15z'></path>
        
        <circle cx='0' cy='0' r='8' stroke='rgb(0,0,155)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        <circle cx='0' cy='-5' r='8' stroke='rgb(0,0,255)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        
        <rect x='-19' y='8' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        <rect x='-13' y='11' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        <rect x='-7' y='14' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        
        </g>
    </svg>
    
    <svg id='glitchedDrive' style='width:200px;height:70px;cursor:pointer;border-radius:5px;'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='134' y='19' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Glitched Drive</text>
        <text x='135' y='38' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Gives pollen, capacity,</text>
        <text x='134' y='52' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'> and bee attack for 3 mins</text>
        <text x='132' y='65' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>in the Robo Challenge.</text>
        <text id='glitchedDrive_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(35,29)'>
        <path fill='rgb(100,100,100)' stroke='black' stroke-width='2' d='M -20 0L0 -15L20 0L0 15z'></path>
        <path fill='rgb(85,85,85)' stroke='black' stroke-width='2' d='M0 25L-20 10L-20 0L0 15z'></path>
        <path fill='rgb(70,70,70)' stroke='black' stroke-width='2' d='M0 25L20 10L20 0L0 15z'></path>
        
        <g transform='translate(-3,2) scale(0.9,0.9)'>
        <circle cx='0' cy='0' r='8' stroke='rgb(195,195,0,0.5)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        <circle cx='0' cy='-5' r='8' stroke='rgb(255,255,0,0.5)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.5)'></circle>
        </g>
        <g transform='translate(3,2) scale(0.9,0.9)'>
        <circle cx='0' cy='0' r='8' stroke='rgb(189, 100, 180,0.6)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        <circle cx='0' cy='-5' r='8' stroke='rgb(247, 131, 235,0.6)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        </g>
        <g transform='translate(0,-2) scale(0.9,0.9)'>
        <circle cx='0' cy='0' r='8' stroke='rgb(0, 190, 190,0.4)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        <circle cx='0' cy='-5' r='8' stroke='rgb(247, 255, 255,0.4)' stroke-width='8' fill='rgb(0,0,0,0)' transform='scale(1,0.75)'></circle>
        </g>
        
        <rect x='-19' y='8' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        <rect x='-13' y='11' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        <rect x='-7' y='14' width='4' height='8' fill='rgb(255,220,50,0.9)' rx='2'></rect>
        
        </g>
    </svg>
    
    <svg id='comfortingVial' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='132' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Comforting Vial</text>
        <text x='133' y='41' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Grants 1 hour of</text>
        <text x='133' y='55' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Comforting Nectar.</text>
        <text id='comfortingVial_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(36,31) scale(0.85,0.85) rotate(-31)'>
        <path stroke='black' stroke-width='2' fill='rgb(0,0,0,0.25)' d='M-6 -15L5 -15C 5 9 13 -1 15 11C 16 15 13 20 13 20C10 30 -10 30 -13 20C -13 20 -16 15 -15 11C -13 -1 -5 9 -5 -15'></path>
        <rect stroke='black' stroke-width='2' fill='rgb(181, 120, 60)' x='-7' y='-22' width='14' height='9' rx='4'></rect>
        
        <path fill='rgb(133, 206, 255)' d='M 7 2C 34 34 -34 34 -7 2'></path>
        <circle cx='0' cy='3' r='6' transform='scale(1,0.55)' fill='rgb(104, 169, 212)'></circle>
        
        <path stroke='black' stroke-width='2.5' fill='rgb(133, 206, 255)' d='M 15 9 C -6 36 37 34 15 9' transform='scale(0.5,0.7) translate(-22,-38) rotate(48)'></path>
        <path stroke='black' stroke-width='2.5' fill='rgb(133, 206, 255)' d='M 15 9 C -6 36 37 34 15 9' transform='scale(0.5,0.6) translate(-43,-16) rotate(39)'></path>
        
        <path fill='rgb(30,30,30)' d='M 22 26C -2 39 0 -1 19 8C 13 5 5 25 22 24' transform='translate(-5,6) scale(0.8,0.75) scale(0.65,0.65)'></path>
        </g>
    </svg>
    
    <svg id='invigoratingVial' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='132' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Invigorating Vial</text>
        <text x='133' y='41' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Grants 1 hour of</text>
        <text x='133' y='55' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Invigorating Nectar.</text>
        <text id='invigoratingVial_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(36,31) scale(0.85,0.85) rotate(-31)'>
        <path stroke='black' stroke-width='2' fill='rgb(0,0,0,0.25)' d='M-6 -15L5 -15C 5 9 13 -1 15 11C 16 15 13 20 13 20C10 30 -10 30 -13 20C -13 20 -16 15 -15 11C -13 -1 -5 9 -5 -15'></path>
        <rect stroke='black' stroke-width='2' fill='rgb(181, 120, 60)' x='-7' y='-22' width='14' height='9' rx='4'></rect>
        
        <path fill='rgb(250, 97, 97)' d='M 7 2C 34 34 -34 34 -7 2'></path>
        <circle cx='0' cy='3' r='6' transform='scale(1,0.55)' fill='rgb(201, 78, 78)'></circle>
        
        <path stroke='black' stroke-width='2.5' fill='rgb(250, 97, 97)' d='M 15 9 C -6 36 37 34 15 9' transform='scale(0.5,0.7) translate(-22,-38) rotate(48)'></path>
        <path stroke='black' stroke-width='2.5' fill='rgb(250, 97, 97)' d='M 15 9 C -6 36 37 34 15 9' transform='scale(0.5,0.6) translate(-43,-16) rotate(39)'></path>
        
        <path fill='rgb(30,30,30)' d='M15 25 C 1 26 5 10 8 8L 12 16L15 0L18 16L 22 8C 27 12 28 27 15 25' transform='translate(-8,7) scale(0.8,0.75) scale(0.75,0.75)'></path>
        <path stroke='rgb(250, 97, 97)' stroke-width='3' d='M15 19 L15 30' transform='translate(-8,7) scale(0.8,0.75) scale(0.75,0.75)'></path>
        
        </g>
    </svg>
    
    <svg id='motivatingVial' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='132' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Motivating Vial</text>
        <text x='133' y='41' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Grants 1 hour of</text>
        <text x='133' y='55' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Motivating Nectar.</text>
        <text id='motivatingVial_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(36,31) scale(0.85,0.85) rotate(-31)'>
        <path stroke='black' stroke-width='2' fill='rgb(0,0,0,0.25)' d='M-6 -15L5 -15C 5 9 13 -1 15 11C 16 15 13 20 13 20C10 30 -10 30 -13 20C -13 20 -16 15 -15 11C -13 -1 -5 9 -5 -15'></path>
        <rect stroke='black' stroke-width='2' fill='rgb(181, 120, 60)' x='-7' y='-22' width='14' height='9' rx='4'></rect>
        
        <path fill='rgb(169, 86, 252)' d='M 7 2C 34 34 -34 34 -7 2'></path>
        <circle cx='0' cy='3' r='6' transform='scale(1,0.55)' fill='rgb(131, 67, 196)'></circle>
        
        <path stroke='black' stroke-width='2.5' fill='rgb(169, 86, 252)' d='M 15 9 C -6 36 37 34 15 9' transform='scale(0.5,0.7) translate(-22,-38) rotate(48)'></path>
        <path stroke='black' stroke-width='2.5' fill='rgb(169, 86, 252)' d='M 15 9 C -6 36 37 34 15 9' transform='scale(0.5,0.6) translate(-43,-16) rotate(39)'></path>
        
        <g transform='translate(-11,2) scale(0.75,0.75)'>
        <path fill='rgb(30,30,30)' d='M 10 20 C 0 1 30 0 20 20'></path>
        <rect fill='rgb(30,30,30)' x='10' y='22' width='10' height='4' rx='2'></rect>
        <path stroke='rgb(169, 86, 252)' fill='rgb(0,0,0,0)' stroke-width='2' d='M 11 15 L 13 12L 15 15L 17 12L19 15'></path>
        </g>
        
        </g>
    </svg>

    <svg id='refreshingVial' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='132' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Refreshing Vial</text>
        <text x='133' y='41' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Grants 1 hour of</text>
        <text x='133' y='55' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Refreshing Nectar.</text>
        <text id='refreshingVial_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(36,31) scale(0.85,0.85) rotate(-31)'>
        <path stroke='black' stroke-width='2' fill='rgb(0,0,0,0.25)' d='M-6 -15L5 -15C 5 9 13 -1 15 11C 16 15 13 20 13 20C10 30 -10 30 -13 20C -13 20 -16 15 -15 11C -13 -1 -5 9 -5 -15'></path>
        <rect stroke='black' stroke-width='2' fill='rgb(181, 120, 60)' x='-7' y='-22' width='14' height='9' rx='4'></rect>
        
        <path fill='rgb(87, 232, 76)' d='M 7 2C 34 34 -34 34 -7 2'></path>
        <circle cx='0' cy='3' r='6' transform='scale(1,0.55)' fill='rgb(71, 179, 61)'></circle>
        
        <path stroke='black' stroke-width='2.5' fill='rgb(87, 232, 76)' d='M 15 9 C -6 36 37 34 15 9' transform='scale(0.5,0.7) translate(-22,-38) rotate(48)'></path>
        <path stroke='black' stroke-width='2.5' fill='rgb(87, 232, 76)' d='M 15 9 C -6 36 37 34 15 9' transform='scale(0.5,0.6) translate(-43,-16) rotate(39)'></path>
        
        <g transform='scale(0.65,0.65) translate(-14,7)'>
        <path fill='rgb(30,30,30)' d='M 15 9 C -6 36 37 34 15 9' transform='scale(1.25,1.25) translate(-3,-5)'></path>
        <path fill='rgb(87, 232, 76)' d='M 15 9 C -6 36 37 34 15 9' transform='scale(0.3,0.3) translate(36,60)'></path>
        <path fill='rgb(87, 232, 76)' d='M 15 9 C -6 36 37 34 15 9' transform='scale(0.3,-0.3) translate(36,-72)'></path>
        </g>
        
        </g>
    </svg>
    
    <svg id='satisfyingVial' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='132' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Satisfying Vial</text>
        <text x='133' y='41' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Grants 1 hour of</text>
        <text x='133' y='55' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Satisfying Nectar.</text>
        <text id='satisfyingVial_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(36,31) scale(0.85,0.85) rotate(-31)'>
        <path stroke='black' stroke-width='2' fill='rgb(0,0,0,0.25)' d='M-6 -15L5 -15C 5 9 13 -1 15 11C 16 15 13 20 13 20C10 30 -10 30 -13 20C -13 20 -16 15 -15 11C -13 -1 -5 9 -5 -15'></path>
        <rect stroke='black' stroke-width='2' fill='rgb(181, 120, 60)' x='-7' y='-22' width='14' height='9' rx='4'></rect>
        
        <path fill='rgb(255, 135, 185)' d='M 7 2C 34 34 -34 34 -7 2'></path>
        <circle cx='0' cy='3' r='6' transform='scale(1,0.55)' fill='rgb(199, 105, 144)'></circle>
        
        <path stroke='black' stroke-width='2.5' fill='rgb(255, 135, 185)' d='M 15 9 C -6 36 37 34 15 9' transform='scale(0.5,0.7) translate(-22,-38) rotate(48)'></path>
        <path stroke='black' stroke-width='2.5' fill='rgb(255, 135, 185)' d='M 15 9 C -6 36 37 34 15 9' transform='scale(0.5,0.6) translate(-43,-16) rotate(39)'></path>
        
        <g transform='scale(0.65,0.65) translate(-14,7)'>
        <circle cx='15' cy='15' r='10' fill='rgb(30,30,30)'></circle>
        <path stroke='rgb(255, 135, 185)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 19 4 C 4 25 26 3 11 27'></path>
        </g>
        
        </g>
    </svg>
    
    <svg id='softWax' style='width:200px;height:70px;cursor:pointer;border-radius:5px;'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='134' y='19' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Soft Wax</text>
        <text x='132' y='35' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>A squishy hunk of</text>
        <text x='135' y='50' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>beeswax. Slightly</text>
        <text x='136' y='64' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>improves a beequip.</text>
        <text id='softWax_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(-4,-2)'>
        <path fill='#fbefb8' stroke='black' stroke-width='1.5' d='M 54 54 C 48 64 23 58 23 52C 20 45 28 49 25 42C 23 40 27 32 29 33C 31 33 27 27 32 25C 45 21 25 23 47 25C 54 30 49 26 53 32C 48 39 57 34 52 42C 58 49 53 44 54 54'></path>
        <path fill='rgb(0,0,0,0)' stroke='rgb(0,0,0,0.3)' stroke-width='2' d='M 47 47 C 48 50 42 53 38 50M 32 49 C 26 52 43 53 37 50M 42 42 C 48 40 42 48 33 41M 39 35 C 41 40 34 39 33 34'></path>
        <circle fill='rgb(0,0,0,0.3)' cx='41' cy='59' r='6.5' transform='scale(1,0.5)'></circle>
        <path fill='rgb(0,0,0,0)' stroke='rgb(0,0,0)' stroke-width='6' d='M 36 20 C 32 19 42 22 40 30' transform='translate(1,0)'></path>
        <path fill='rgb(0,0,0,0)' stroke='rgb(255, 255, 255)' stroke-width='3' d='M 36 20 C 32 19 42 22 40 30' transform='translate(1,0)'></path>
        </g>
        
    </svg>
    
    <svg id='hardWax' style='width:200px;height:70px;cursor:pointer;border-radius:5px;'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='134' y='19' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Hard Wax</text>
        <text x='136' y='35' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>A firm hunk of beeswax.</text>
        <text x='135' y='50' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Has a 60% chance to</text>
        <text x='136' y='64' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>improve a beequip.</text>
        <text id='hardWax_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(-4,-2)'>
        <path fill='#e26a3a' stroke='black' stroke-width='1.5' d='M 56 54 L 49 58L 33 58L 25 54L 25 42L 28 35L 28 23L 38 15L 46 18L 52 24L 52 35L 56 47L 56 55' transform='scale(0.9,0.9) translate(4,4)'></path>
        <path fill='rgb(0,0,0,0)' stroke='rgb(0,0,0,0.2)' stroke-width='3' d='M 33 57 L 33 44M 46 48 L 46 33M 38 40 L 38 30'></path>
        <path fill='rgb(0,0,0,0)' stroke='rgb(0,0,0)' stroke-width='6' d='M 36 20 C 32 19 42 22 40 30' transform='translate(0,-5)'></path>
        <circle fill='rgb(0,0,0,0.3)' cx='40' cy='42' r='6' transform='scale(1,0.6)'></circle>
        <path fill='rgb(0,0,0,0)' stroke='rgb(255, 255, 255)' stroke-width='3' d='M 36 20 C 32 19 42 22 40 30' transform='translate(0,-5)'></path>
        </g>
        
    </svg>
    
    <svg id='causticWax' style='width:200px;height:70px;cursor:pointer;border-radius:5px;'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='134' y='19' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Caustic Wax</text>
        <text x='136' y='35' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>A glowing hunk of wax. Has</text>
        <text x='135' y='50' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>a 25% chance to improve, </text>
        <text x='136' y='64' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>or else DESTROY a beequip.</text>
        <text id='causticWax_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(-4,-2)'>
        <path fill='#7cec88' stroke='black' stroke-width='1.5' d='M 54 54 C 48 64 23 58 23 52C 23 45 28 49 25 37C 23 31 28 28 27 26C 23 18 49 16 50 26C 48 36 54 31 53 40C 53 50 58 44 54 54'></path>
        <circle fill='rgb(205, 242, 41)' cx='31' cy='50' r='3'></circle><circle fill='rgb(205, 242, 41)' cx='40' cy='42' r='2'></circle><circle fill='rgb(205, 242, 41)' cx='30' cy='36' r='2'></circle><circle fill='rgb(205, 242, 41)' cx='46' cy='32' r='2'></circle><circle fill='rgb(205, 242, 41)' cx='46' cy='53' r='2'></circle>
        <circle fill='rgb(0,0,0,0.3)' cx='39' cy='51' r='7' transform='scale(1,0.5)'></circle>
        <path fill='rgb(0,0,0,0)' stroke='rgb(0,0,0)' stroke-width='6' d='M 36 20 C 32 19 42 22 40 30' transform='translate(0,-4)'></path>
        <path fill='rgb(0,0,0,0)' stroke='rgb(255, 255, 255)' stroke-width='3' d='M 36 20 C 32 19 42 22 40 30' transform='translate(0,-4)'></path>
        </g>
        
    </svg>
    
    <svg id='swirledWax' style='width:200px;height:70px;cursor:pointer;border-radius:5px;'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='134' y='19' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Swirled Wax</text>
        <text x='136' y='35' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>A spiraled hunk of wax.</text>
        <text x='135' y='49' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Can be used to reroll the</text>
        <text x='136' y='64' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>stats of a beequip.</text>
        <text id='swirledWax_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(-4,-2)'>
        <path fill='rgb(255,255,120)' stroke='black' stroke-width='1.5' d='M 54 54 C 31 64 23 58 24 52C 24 43 28 49 25 37C 23 31 28 28 27 26C 23 18 49 16 50 26C 48 36 54 31 51 40C 53 50 57 44 54 54'></path>
        
        <circle fill='rgb(0,0,0,0.3)' cx='39' cy='51' r='7' transform='scale(1,0.5)'></circle>
        <path fill='rgb(0,0,0,0)' stroke='rgb(0,0,0)' stroke-width='6' d='M 36 20 C 32 19 42 22 40 30' transform='translate(-1,-4)'></path>
        <path fill='rgb(0,0,0,0)' stroke='rgb(255, 255, 255)' stroke-width='3' d='M 36 20 C 32 19 42 22 40 30' transform='translate(-1,-4)'></path>
        <path fill='rgb(0,0,0,0)' stroke='rgb(0,0,0,0.3)' stroke-width='2' d='M 33 54 C 26 51 30 44 32 46C 40 48 31 51 33 50'></path>
        <path fill='rgb(0,0,0,0)' stroke='rgb(0,0,0,0.3)' stroke-width='2' d='M 31 54 C 26 51 27 44 35 46C 40 48 31 51 33 50' transform=' translate(0,109) scale(1.4,1.4) rotate(-114)'></path>
        </g>
        
    </svg>

    <svg id='turpentine' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='132' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Turpentine</text>
        <text x='133' y='38' style='font-family:trebuchet ms;font-size:11.5px;' fill='rgb(0,0,0)' text-anchor='middle'>A strong solvent that</text>
        <text x='133' y='52' style='font-family:trebuchet ms;font-size:11.5px;' fill='rgb(0,0,0)' text-anchor='middle'>can remove all waxes</text>
        <text x='133' y='65' style='font-family:trebuchet ms;font-size:11.5px;' fill='rgb(0,0,0)' text-anchor='middle'>from a bee's beequip.</text>
        <text id='turpentine_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(36,33) scale(0.85,0.85)'>
        <path stroke='black' stroke-width='1.5' fill='rgb(186, 128, 41)' d='M-10 -15L10 -15C 21 -9 19 -4 15 15C15 25 -15 25 -15 15C-19 -4 -21 -9 -10 -15' transform='scale(0.9,1)'></path>
        <rect stroke='black' stroke-width='1.5' fill='rgb(133, 133, 133)' x='-11' y='-23' width='22' height='8' rx='2'></rect>
        <path stroke='rgb(200,0,0)' stroke-width='6' fill='rgb(0,0,0,0)' d='M -16 0 C -10 3 10 3 16 0'></path>
        <path stroke='rgb(255,180,0)' stroke-width='3' fill='rgb(0,0,0,0)' d='M -5 -3 C -10 3 8 -8 5 -2M 0 -3 C 0 5 3 -2 -1 9'></path>
        </g>
    </svg>
    
    <svg id='basicEgg' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Basic Egg</text>
        <text x='130' y='39' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Hatches into a </text>
        <text x='130' y='54' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>basic bee!</text>
        <text id='basicEgg_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        <path fill='rgb(255,255,0)' stroke='rgb(0,0,0)' stroke-width='1.5' d='M35 15C 20 17 10 55 35 55M35 15C 50 17 60 55 35 55'></path>
        <path fill='rgb(0,0,0)' d='M20 30 C 20 40 50 40 50 30L50 40C50 50 20 50 20 40'></path>
        
        <path fill='rgb(0,0,0,0.3)' d='M47 25C 57 56 35 60 23 50C 32 48 41 50 50 35'></path>
        
    </svg>
    
    <svg id='silverEgg' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Silver Egg</text>
        <text x='130' y='39' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Hatches into a </text>
        <text x='130' y='54' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>special bee!</text>
        <text id='silverEgg_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <path fill='rgb(200,200,200)' stroke='rgb(0,0,0)' stroke-width='1.5' d='M35 15C 20 17 10 55 35 55M35 15C 50 17 60 55 35 55'></path><path stroke='rgb(255,255,255)' stroke-width='2' fill='rgb(0,0,0,0)' d='M 25 25C 25 25 40 25 40 18M 22 34C 25 35 40 35 47 28M 22 44C 25 45 40 45 49 38M 28 52C 25 53 40 53 47 47'></path>
        
        <path fill='rgb(0,0,0,0.3)' d='M47 25C 57 56 35 60 23 50C 32 48 41 50 50 35'></path>
    </svg>
    
    <svg id='goldEgg' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Gold Egg</text>
        <text x='130' y='36' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Hatches into an </text>
        <text x='130' y='50' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>epic, legendary, or</text><text x='130' y='65' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>mythic bee!</text>
        
        <text id='goldEgg_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <path fill='rgb(255,220,70)' stroke='rgb(0,0,0)' stroke-width='1.5' d='M35 15C 20 17 10 55 35 55M35 15C 50 17 60 55 35 55'></path><path stroke='rgb(235,205,0)' stroke-width='2' fill='rgb(0,0,0,0)' d='M 25 25C 25 25 40 25 40 18M 22 34C 25 35 40 35 47 28M 22 44C 25 45 40 45 49 38M 28 52C 25 53 40 53 47 47'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26'></path>
        <path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(52,17) rotate(100) scale(0.5,0.5)'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(20,22) rotate(28) scale(0.5,0.5)'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(57,52) rotate(168) scale(0.5,0.5)'></path>
        
        <path fill='rgb(0,0,0,0.2)' d='M47 25C 57 56 35 60 23 50C 32 48 41 50 50 35'></path>
    </svg>
    
    <svg id='diamondEgg' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Diamond Egg</text>
        <text x='130' y='36' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Hatches into a </text>
        <text x='130' y='50' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'> legendary or</text><text x='130' y='65' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>mythic bee!</text>
        
        <text id='diamondEgg_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <path fill='rgb(200,255,255)' stroke='rgb(0,0,0)' stroke-width='1.5' d='M35 15C 20 17 10 55 35 55M35 15C 50 17 60 55 35 55'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26'></path>
        <path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(52,17) rotate(100) scale(0.5,0.5)'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(20,22) rotate(28) scale(0.5,0.5)'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(57,52) rotate(168) scale(0.5,0.5)'></path><path fill='rgb(0,0,0,0.08)' d='M35 26C 28 30 28 45 35 45M35 26C 42 30 42 45 35 45'></path>
        
        <path fill='rgb(0,0,0,0.2)' d='M47 25C 57 56 35 60 23 50C 32 48 41 50 50 35'></path>
    </svg>
    
    <svg id='mythicEgg' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Mythic Egg</text>
        <text x='130' y='40' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Always hatches into</text>
        <text x='130' y='53' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>a mythic bee!</text>
        
        <text id='mythicEgg_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <path fill='rgb(163, 91, 218)' stroke='rgb(0,0,0)' stroke-width='1.5' d='M35 15C 20 17 10 55 35 55M35 15C 50 17 60 55 35 55'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26'></path>
        <path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(49,17) rotate(100) scale(0.35,0.35)'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(22,20) rotate(28) scale(0.35,0.5)'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(57,52) rotate(168) scale(0.45,0.45)'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(25,59) rotate(263) scale(0.35,0.35)'></path>
        <path fill='rgb(0,0,0)' d='M20 30 C 20 40 50 40 50 30L50 40C50 50 20 50 20 40'></path>
        <text fill='rgb(0,0,0)' x='21' y='45' style='font-family:comic sans ms' transform='scale(1,0.7) scale(1.3,1.3)'>M</text>
        <text fill='rgb(255,255,255)' x='29' y='51' style='font-family:comic sans ms' transform='scale(1,0.8)'>M</text>
        
        <path fill='rgb(0,0,0,0.3)' d='M47 25C 57 56 35 60 23 50C 32 48 41 50 50 35'></path>
    </svg>
    
    <svg id='giftedSilverEgg' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:15px;' fill='rgb(0,0,0)' text-anchor='middle'>Gifted Silver Egg</text>
        <text x='130' y='39' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Hatches into a </text>
        <text x='130' y='54' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>gifted special bee!</text>
        <text id='giftedSilverEgg_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <path fill='rgb(200,200,200)' stroke='rgb(0,0,0)' stroke-width='1.5' d='M35 15C 20 17 10 55 35 55M35 15C 50 17 60 55 35 55'></path><path stroke='rgb(255,255,255)' stroke-width='2' fill='rgb(0,0,0,0)' d='M 25 25C 25 25 40 25 40 18M 22 34C 25 35 40 35 47 28M 22 44C 25 45 40 45 49 38M 28 52C 25 53 40 53 47 47'></path>
        
        <path fill='rgb(0,0,0,0.3)' d='M47 25C 57 56 35 60 23 50C 32 48 41 50 50 35'></path>
        
        <path fill='rgb(255, 255, 89)' d='M5.71,16.85L8.23,26.33L0.00,21.00L-8.23,26.33L-5.71,16.85L-13.31,10.67L-3.53,10.15L-0.00,1.00L3.53,10.15L13.31,10.67L5.71,16.85z' transform='translate(36,28) scale(0.6,0.6)' stroke='black' stroke-width='1.5'></path>
        
    </svg>
    
    <svg id='giftedGoldEgg' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:15px;' fill='rgb(0,0,0)' text-anchor='middle'>Gifted Gold Egg</text>
        <text x='130' y='37' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Hatches into a gifted</text>
        <text x='130' y='51' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>epic, legendary, or</text><text x='130' y='65' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>mythic bee!</text>
        
        <text id='giftedGoldEgg_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <path fill='rgb(255,220,70)' stroke='rgb(0,0,0)' stroke-width='1.5' d='M35 15C 20 17 10 55 35 55M35 15C 50 17 60 55 35 55'></path><path stroke='rgb(235,205,0)' stroke-width='2' fill='rgb(0,0,0,0)' d='M 25 25C 25 25 40 25 40 18M 22 34C 25 35 40 35 47 28M 22 44C 25 45 40 45 49 38M 28 52C 25 53 40 53 47 47'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26'></path>
        <path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(52,17) rotate(100) scale(0.5,0.5)'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(20,22) rotate(28) scale(0.5,0.5)'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(57,52) rotate(168) scale(0.5,0.5)'></path>
        
        <path fill='rgb(0,0,0,0.2)' d='M47 25C 57 56 35 60 23 50C 32 48 41 50 50 35'></path>
        
        <path fill='rgb(255, 255, 89)' d='M5.71,16.85L8.23,26.33L0.00,21.00L-8.23,26.33L-5.71,16.85L-13.31,10.67L-3.53,10.15L-0.00,1.00L3.53,10.15L13.31,10.67L5.71,16.85z' transform='translate(36,29) scale(0.6,0.6)' stroke='black' stroke-width='1.5'></path>
        
    </svg>
    
    <svg id='giftedDiamondEgg' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:13px;' fill='rgb(0,0,0)' text-anchor='middle'>Gifted Diamond Egg</text>
        <text x='130' y='37' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Hatches into a gifted</text>
        <text x='130' y='51' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'> legendary or</text><text x='130' y='65' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>mythic bee!</text>
        
        <text id='giftedDiamondEgg_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <path fill='rgb(200,255,255)' stroke='rgb(0,0,0)' stroke-width='1.5' d='M35 15C 20 17 10 55 35 55M35 15C 50 17 60 55 35 55'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26'></path>
        <path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(52,17) rotate(100) scale(0.5,0.5)'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(20,22) rotate(28) scale(0.5,0.5)'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(57,52) rotate(168) scale(0.5,0.5)'></path><path fill='rgb(0,0,0,0.08)' d='M35 26C 28 30 28 45 35 45M35 26C 42 30 42 45 35 45'></path>
        
        <path fill='rgb(0,0,0,0.2)' d='M47 25C 57 56 35 60 23 50C 32 48 41 50 50 35'></path>
        <path fill='rgb(255, 255, 89)' d='M5.71,16.85L8.23,26.33L0.00,21.00L-8.23,26.33L-5.71,16.85L-13.31,10.67L-3.53,10.15L-0.00,1.00L3.53,10.15L13.31,10.67L5.71,16.85z' transform='translate(36,28) scale(0.6,0.6)' stroke='black' stroke-width='1.5'></path>
    </svg>
    
    <svg id='giftedMythicEgg' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:14.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Gifted Mythic Egg</text>
        <text x='130' y='40' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Always hatches into</text>
        <text x='130' y='53' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>a gifted mythic bee!</text>
        
        <text id='giftedMythicEgg_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <path fill='rgb(163, 91, 218)' stroke='rgb(0,0,0)' stroke-width='1.5' d='M35 15C 20 17 10 55 35 55M35 15C 50 17 60 55 35 55'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26'></path>
        <path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(49,17) rotate(100) scale(0.35,0.35)'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(22,20) rotate(28) scale(0.35,0.5)'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(57,52) rotate(168) scale(0.45,0.45)'></path><path stroke='rgb(255,255,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 35 18C 30 20 28 22 27 26M 25 16C 30 20 28 22 37 26' transform='translate(25,59) rotate(263) scale(0.35,0.35)'></path>
        <path fill='rgb(0,0,0)' d='M20 30 C 20 40 50 40 50 30L50 40C50 50 20 50 20 40'></path>
        
        <path fill='rgb(0,0,0,0.3)' d='M47 25C 57 56 35 60 23 50C 32 48 41 50 50 35'></path>
        <path fill='rgb(255, 255, 89)' d='M5.71,16.85L8.23,26.33L0.00,21.00L-8.23,26.33L-5.71,16.85L-13.31,10.67L-3.53,10.15L-0.00,1.00L3.53,10.15L13.31,10.67L5.71,16.85z' transform='translate(36,28) scale(0.65,0.65)' stroke='black' stroke-width='1.5'></path>
    </svg>
    
    <svg id='starEgg' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Star Egg</text>
        <text x='130' y='36' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Hatches into a </text>
        <text x='130' y='49' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>gifted bee you</text>
        <text x='130' y='61' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>don't own!</text>
        <text id='starEgg_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <path fill='rgb(250,250,250)' stroke='rgb(0,0,0)' stroke-width='1.5' d='M35 15C 20 17 10 55 35 55M35 15C 50 17 60 55 35 55'></path>
        
        <path fill='rgb(0,0,0,0.25)' d='M47 25C 57 56 35 60 23 50C 32 48 41 50 50 35'></path>
        
        <path fill='rgb(255, 255, 89)' d='M5.71,16.85L8.23,26.33L0.00,21.00L-8.23,26.33L-5.71,16.85L-13.31,10.67L-3.53,10.15L-0.00,1.00L3.53,10.15L13.31,10.67L5.71,16.85z' transform='translate(36,28) scale(0.6,0.6)' stroke='black' stroke-width='1.5'></path>
        
    </svg>
        
    <svg id='royalJelly' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Royal Jelly</text>
        <text x='130' y='39' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'>Transforms a bee</text>
        <text x='130' y='54' style='font-family:trebuchet ms;font-size:12px;' fill='rgb(0,0,0)' text-anchor='middle'> into a special bee!</text>
        <text id='royalJelly_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='scale(1.1,1.1) translate(-6,-6)'>
        <rect x='25' y='30' width='25' height='25' rx='6' fill='rgb(94, 235, 214)' stroke='black' stroke-width='1.5'></rect>
        
        <path stroke='rgb(168, 50, 201)' stroke-width='2' fill='rgb(0,0,0,0)' d='M 33 49L 35 36C 45 39 39 44 34 43L40 48'></path>
        <path stroke='black' stroke-width='1.5' fill='rgb(245, 202, 12)' d='M 26 32L 25 22L 33 27L 37 17L 41 27L 48 22L 48 32z'></path>
        <circle cx='25' cy='23' r='3' fill='rgb(245, 229, 12)' stroke='black' stroke-width='1.5'></circle>
        <circle cx='48' cy='23' r='3' fill='rgb(245, 229, 12)' stroke='black' stroke-width='1.5'></circle>
        <circle cx='37' cy='15' r='3.5' fill='rgb(245, 229, 12)' stroke='black' stroke-width='1.5'></circle>
        </g>
    </svg>
    
    <svg id='starJelly' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Star Jelly</text>
        <text x='130' y='39' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'>Transforms a bee</text>
        <text x='130' y='53' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'> into a gifted </text>
        <text x='130' y='66' style='font-family:trebuchet ms;font-size:11px;' fill='rgb(0,0,0)' text-anchor='middle'> special bee!</text>
        <text id='starJelly_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='scale(1.1,1.1) translate(-6,-6)'>
        <rect x='25' y='30' width='25' height='25' rx='6' fill='rgb(94, 235, 214)' stroke='black' stroke-width='1.5'></rect>
        <path stroke='black' stroke-width='2' fill='rgb(245, 202, 12)' d='M 26 32L 25 22L 33 27L 37 17L 41 27L 48 22L 48 32z'></path>
        <circle cx='25' cy='23' r='3' fill='rgb(245, 229, 12)' stroke='black' stroke-width='1.5'></circle>
        <circle cx='48' cy='23' r='3' fill='rgb(245, 229, 12)' stroke='black' stroke-width='1.5'></circle>
        <circle cx='37' cy='15' r='3.5' fill='rgb(245, 229, 12)' stroke='black' stroke-width='1.5'></circle>
        <path fill='rgb(255, 255, 89)' d='M5.71,16.85L8.23,26.33L0.00,21.00L-8.23,26.33L-5.71,16.85L-13.31,10.67L-3.53,10.15L-0.00,1.00L3.53,10.15L13.31,10.67L5.71,16.85z' transform='translate(38,33) scale(0.6,0.6)' stroke='black' stroke-width='1.5'></path>
        </g>
    </svg>

    <svg id='paperPlanter' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Paper Planter</text>
        <text x='132' y='35' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>A biodegradeable pot</text>
        <text x='132' y='50' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>that grows in 10m, giving</text>
        <text x='132' y='65' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>rewards & nectar boosts!</text>
        <text id='paperPlanter_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(-2,12)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(209, 151, 57)' d='M 21 44L 37 50 L 53 44 L 53 11L 38 3L 21 11z'></path>
        
        <path fill='rgb(0,0,0,0.1)' d='M 37 50 L 53 44 L 53 11L 38 3z'></path>
        
        <path stroke='black' stroke-width='1' fill='rgb(89, 62, 19)' d='M 21 12L 38 3 L 53 11 L 38 19z'></path>
        
        <circle stroke='black' stroke-width='1' cx='50' cy='4' r='9' fill='rgb(0,190,50)' transform='scale(0.75,1)'></circle>
        
        <path stroke='rgb(153, 109, 38)' stroke-width='2' fill='rgb(0,0,0,0)' d='M 38 26L 22 20M 52 19L 38 26M 45 22L 39 48M 45 22L 52 44M 30 22L 38 50M 30 22L 22 45'></path>
        
        </g>
        
    </svg>

    <svg id='plasticPlanter' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Plastic Planter</text>
        <text x='132' y='35' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>A reuseable planter that</text>
        <text x='134' y='50' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>grows in 17m, giving extra</text>
        <text x='132' y='65' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>rewards & nectar boosts!</text>
        <text id='plasticPlanter_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(-4,1)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(240, 240, 45)' d='M 55 50 C 45 56 26 53 27 48L 24 30L 24 23C 31 16 47 13 58 22L 58 34z'></path>
        <circle cx='42' cy='59' r='11' fill='rgb(0,0,0,0.3)' transform='scale(1,0.4)'></circle>
        <path stroke='rgb(0,0,0,0.1)' fill='rgb(0,0,0,0)' stroke-width='3' d='M 25 33 C 36 37 38 40 57 35'></path>
        
        </g>
        
    </svg>

    <svg id='candyPlanter' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='130' y='18' style='font-family:trebuchet ms;font-size:16.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Candy Planter</text>
        <text x='132' y='35' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Alone, grows in about</text>
        <text x='132' y='50' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>25m, storing pollen and</text>
        <text x='132' y='65' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>gives bonus gumdrops!</text>
        <text id='candyPlanter_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(-4,1)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(255, 145, 250)' d='M 55 50 C 45 56 26 53 27 48L 24 30L 24 23C 31 16 47 13 58 22L 58 34z'></path>
        <circle cx='42' cy='59' r='11' fill='rgb(0,0,0,0.3)' transform='scale(1,0.4)'></circle>
        <path stroke='rgb(0,0,0,0.1)' fill='rgb(0,0,0,0)' stroke-width='3' d='M 25 33 C 36 37 38 40 57 35'></path>
        
        <path stroke='rgb(255,255,255)' fill='rgb(0,0,0,0)' stroke-width='3.5' d='M 27 47 C 34 46 41 42 41 39M 34 52 C 56 44 49 42 51 38'></path>
        
        <circle cx='30' cy='30' r='2.5' fill='rgb(0,205,0)'></circle><circle cx='38' cy='33' r='2.5' fill='rgb(255,255,0)'></circle><circle cx='48' cy='32' r='2.5' fill='rgb(0,105,255)'></circle><circle cx='54' cy='28' r='2' fill='rgb(255,0,0)'></circle>
        
        </g>
        
    </svg>
    
    <svg id='redClayPlanter' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='133' y='19' style='font-family:trebuchet ms;font-size:16px;' fill='rgb(0,0,0)' text-anchor='middle'>Red Clay Planter</text>
        <text x='132' y='35' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Grows in 32m, but faster</text>
        <text x='135' y='50' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>by red flowers. Gives more</text>
        <text x='136' y='65' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>red ingredients and nectar!</text>
        <text id='redClayPlanter_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(-4,1)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(255, 69, 69)' d='M 55 50 C 45 56 26 53 27 48L 24 30L 24 23C 31 16 47 13 58 22L 58 34z'></path>
        <circle cx='42' cy='59' r='11' fill='rgb(0,0,0,0.3)' transform='scale(1,0.4)'></circle>
        <path stroke='rgb(0,0,0,0.1)' fill='rgb(0,0,0,0)' stroke-width='3' d='M 25 33 C 36 37 38 40 57 35'></path>
        
        <circle cx='33' cy='43' r='2' fill='rgb(0,0,0,0.3)'></circle>
        <circle cx='42' cy='46' r='2' fill='rgb(0,0,0,0.3)'></circle><circle cx='51' cy='43' r='2' fill='rgb(0,0,0,0.3)'></circle>
        </g>
        
        
    </svg>

    <svg id='blueClayPlanter' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='134' y='19' style='font-family:trebuchet ms;font-size:15.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Blue Clay Planter</text>
        <text x='132' y='35' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Grows in 32m, but faster</text>
        <text x='135' y='50' style='font-family:trebuchet ms;font-size:10px;' fill='rgb(0,0,0)' text-anchor='middle'>by blue flowers. Gives more</text>
        <text x='136' y='65' style='font-family:trebuchet ms;font-size:10px;' fill='rgb(0,0,0)' text-anchor='middle'>blue ingredients and nectar!</text>
        <text id='blueClayPlanter_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(-4,1)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(89, 71, 255)' d='M 55 50 C 45 56 26 53 27 48L 24 30L 24 23C 31 16 47 13 58 22L 58 34z'></path>
        <circle cx='42' cy='59' r='11' fill='rgb(0,0,0,0.3)' transform='scale(1,0.4)'></circle>
        <path stroke='rgb(0,0,0,0.1)' fill='rgb(0,0,0,0)' stroke-width='3' d='M 25 33 C 36 37 38 40 57 35'></path>
        
        <circle cx='33' cy='43' r='2' fill='rgb(0,0,0,0.3)'></circle>
        <circle cx='42' cy='46' r='2' fill='rgb(0,0,0,0.3)'></circle><circle cx='51' cy='43' r='2' fill='rgb(0,0,0,0.3)'></circle>
        </g>
        
    </svg>

    <svg id='tackyPlanter' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='134' y='19' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Tacky Planter</text>
        <text x='132' y='35' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Grows in 39m, but faster</text>
        <text x='135' y='50' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>near the hives. Gives more</text>
        <text x='136' y='65' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>dice & sunflower seeds! </text>
        <text id='tackyPlanter_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(-2,2) rotate(-4)'>
        
        <circle cx='35' cy='46' r='16' stroke='black' stroke-width='1.5' fill='rgb(188, 222, 153)' ></circle>
        <path stroke='black' stroke-width='1.5' fill='rgb(250,250,250)' d='M 21 33 C 20 39 50 39 49 33L49 25C49 16 21 16 21 25z'></path>
        <circle cx='35' cy='60' r='10' fill='rgb(0,0,0,0.4)' transform='scale(1,0.4)'></circle>
        <path stroke='rgb(225,225,0)' stroke-width='4' fill='rgb(0,0,0,0)' d='M 26 36 L 26 27M 35 36 L 35 29M 44 36 L 44 27'></path>
        <path stroke='rgb(255,120,0)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 26 59 L 35 38M 44 59 L 35 38'></path>
        <path stroke='rgb(0,100,240)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 21 38 L 35 62M 35 62 L 49 38'></path>
        <circle cx='35' cy='25' r='1.5' fill='rgb(220,0,0)' transform='scale(1,2)'></circle><circle cx='23' cy='25' r='1' fill='rgb(220,0,0)' transform='scale(1,2)'></circle><circle cx='47' cy='25' r='1' fill='rgb(220,0,0)' transform='scale(1,2)'></circle>
        </g>
        
    </svg>

    <svg id='pesticidePlanter' style='width:200px;height:70px;cursor:pointer;border-radius:5px;'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='134' y='19' style='font-family:trebuchet ms;font-size:16px;' fill='rgb(0,0,0)' text-anchor='middle'>Pesticide Planter</text>
        <text x='132' y='35' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Grows in 45m, but faster</text>
        <text x='135' y='50' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>in the 5 bee zone. Gives</text>
        <text x='136' y='63' style='font-family:trebuchet ms;font-size:10px;' fill='rgb(0,0,0)' text-anchor='middle'>more bitter and neonberries!</text>
        <text id='pesticidePlanter_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(14,18)'>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(84, 222, 98)' d='M20 40 C 25 45 45 45 50 40L60 30L50 18C45 10 25 10 20 18L10 30z' transform='scale(0.6,1)'></path>
        <path  fill='rgb(0,0,0,0.2)' d='M20 40 C 25 45 45 45 50 40L60 30C 44 38 30 41 10 30z' transform='scale(0.6,1)'></path>
        
        <g transform='translate(2,-18) scale(0.9,0.9)'>
            
            <path stroke='black' stroke-width='1.5' fill='rgb(84, 222, 98)' d='M20 40 C 25 45 45 45 50 40L60 30L50 18C45 10 25 10 20 18L10 30z' transform='scale(0.6,1)'></path>
        <path  fill='rgb(0,0,0,0.2)' d='M20 40 C 25 45 45 45 50 40L60 30C 44 38 30 41 10 30z' transform='scale(0.6,1)'></path>
        
        </g>
        
        <path stroke='rgb(84, 222, 98)' stroke-width='3' fill='rgb(0,0,0,0)' d='M21 18 C 25 24 45 23 49 18' transform='scale(0.6,1)'></path>
        
        <path stroke='black' stroke-width='1' fill='rgb(255,255,0)' d='M 22 23 L 28 32L 16 32z'></path>
        <path stroke='black' stroke-width='1.5' fill='rgb(0,0,0,0)' d='M 22 26 L 22 28M 19 30.5 L 20.5 29.5M 25.5 30.5 L 23.5 29.5'></path>
        <circle fill='rgb(0,0,0)' cx='22' cy='29.25' r='0.8'></circle>
        
        <g transform='translate(2,-18) scale(0.9,0.9)'>
            
        <path stroke='black' stroke-width='1' fill='rgb(255,255,0)' d='M 22 23 L 28 32L 16 32z'></path>
        <path stroke='black' stroke-width='1.5' fill='rgb(0,0,0,0)' d='M 22 26 L 22 28M 19 30.5 L 20.5 29.5M 25.5 30.5 L 23.5 29.5'></path>
        <circle fill='rgb(0,0,0)' cx='22' cy='29.25' r='0.8'></circle>
        
        </g>
        <circle fill='rgb(0,0,0,0.3)' cx='21' cy='-4' r='5' transform='scale(1,0.6)'></circle>
        </g>
    </svg>

    <svg id='petalPlanter' style='width:200px;height:70px;cursor:pointer;border-radius:5px'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='134' y='19' style='font-family:trebuchet ms;font-size:17px;' fill='rgb(0,0,0)' text-anchor='middle'>Petal Planter</text>
        <text x='132' y='35' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Grows in 51m, but faster</text>
        <text x='135' y='50' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>by white flowers. Gives</text>
        <text x='136' y='65' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>bonus glitter and dice!</text>
        <text id='petalPlanter_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <g transform='translate(0,-4)'>
        
        <circle cx='35' cy='49' r='16' stroke='black' stroke-width='1.5' fill='rgb(240, 240, 178)' ></circle>
        <path stroke='black' stroke-width='1.5' fill='rgb(240, 240, 178)' d='M 21 33 C 20 39 50 39 49 33L49 25C49 16 21 16 21 25z'></path>
        <circle cx='35' cy='60' r='10' fill='rgb(0,0,0,0.4)' transform='scale(1,0.4)'></circle>
        <circle cx='35' cy='42' r='3' fill='rgb(0,200,80)' transform='scale(1,1.25)'></circle><circle cx='25' cy='42' r='2' fill='rgb(255,255,0)' transform='scale(1,1.25)'></circle><circle cx='45' cy='42' r='2' fill='rgb(255,255,0)' transform='scale(1,1.25)'></circle>
        <path stroke='black' stroke-width='1' fill='rgb(255,255,255)' d='M 30 30 C 22 45 30 37 35 47C 37 37 44 45 40 30' transform='translate(-7,-8) scale(1.2,1.2)'></path>
        <path stroke='black' stroke-width='1' fill='rgb(255,255,255)' d='M 30 30 C 22 45 30 37 35 47C 37 37 44 45 40 30' transform='translate(8,-26) scale(1.2,1.2) rotate(30)'></path><path stroke='black' stroke-width='1' fill='rgb(255,255,255)' d='M 30 30 C 22 45 30 37 35 47C 37 37 44 45 40 30' transform='translate(-11,16) scale(1.2,1.2) rotate(-30)'></path>
        
        </g>
        
    </svg>

    <svg id='plentyPlanter' style='width:200px;height:70px;cursor:pointer;border-radius:5px;'>
        
        <rect width='200' height='70' fill='rgb(255,255,255)'></rect>
        <rect width='70' height='70' fill='rgb(225,225,225)'></rect>
        <text x='134' y='19' style='font-family:trebuchet ms;font-size:16px;' fill='rgb(0,0,0)' text-anchor='middle'>Planter O' Plenty</text>
        <text x='132' y='35' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>Grows in 60m, but faster</text>
        <text x='135' y='50' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>in end-game fields. Gives</text>
        <text x='136' y='64' style='font-family:trebuchet ms;font-size:10.5px;' fill='rgb(0,0,0)' text-anchor='middle'>many bonus rare items!</text>
        <text id='plentyPlanter_amount' x='67' y='67' style='font-family:calibri;font-size:14px;' fill='rgb(0,0,0)' text-anchor='end'></text>
        
        <path stroke='black' stroke-width='1.5' fill='rgb(255, 210, 0)' d='M 25 51 C 20 54 40 54 40 50C 37 46 35 42 37 33C 44 34 51 27 51 20C 42 9 14 14 13 20C 13 27 19 35 25 33C 31 36 28 51 24 52'></path>
        <path stroke='black' stroke-width='1.5' fill='rgb(0,0,0,0)' d='M 14 23C 5 18 6 36 17 31M 50 23C 60 18 55 37 46 31'></path>
        <path stroke='rgb(255,0,0)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 15 27C 22 25 20 29 31 26'></path>
        <path stroke='rgb(0,0,255)' stroke-width='3' fill='rgb(0,0,0,0)' d='M 49 26C 49 25 42 29 32 26'></path>
        <text x='29' y='34' fill='rgb(255,255,255)' stroke='rgb(0,255,0)' stroke-width='0.5' style='font-family:comic sans ms;font-size:17px;'>P</text>
        
        <circle cx='32' cy='36' r='13' stroke-width='7' stroke='rgb(102, 179, 242)' fill='rgb(135, 110, 0)' transform='scale(1,0.4)'></circle>
        
    </svg>

</div>

<div class='uiPage' style='position:absolute;margin:0px;z-index:-2;overflow-y:auto;overflow-x:hidden;width:200px;height:455px;background-color:rgba(195,195,195,0.9);margin-top:140px;display:none;padding:3px;font-size:13px;'></div>

<div class='uiPage' style='position:absolute;margin:0px;z-index:-2;overflow-y:auto;overflow-x:hidden;width:200px;height:455px;background-color:rgba(195,195,195,0.9);margin-top:140px;display:none;padding:3px;font-size:13px;'></div>

<div class='uiPage' style='position:absolute;margin:0px;z-index:-2;overflow-y:auto;overflow-x:hidden;width:200px;height:455px;background-color:rgba(195,195,195,0.9);margin-top:140px;display:none;padding:3px;font-size:13px;'>

    <div style="cursor:pointer;margin-top:5px;background-color:rgb(170,170,170);font-size:23px;border-radius:3px;color:rgb(0,0,0);text-align:center" id="saveGame">Save Game</div>
    
    <div style="cursor:pointer;margin-top:5px;background-color:rgb(170,170,170);font-size:23px;border-radius:3px;color:rgb(0,0,0);text-align:center" id="resetChar">Respawn</div>
    
    <div style="cursor:pointer;margin-top:10px;background-color:rgb(170,170,170);font-size:15px;border-radius:3px;" id="togglePollenText"></div>
    
    <div style="cursor:pointer;margin-top:5px;background-color:rgb(170,170,170);font-size:15px;border-radius:3px;" id="togglePollenAbv"></div>

    <div id='statString'></div>

    <div style='font-size:17px;text-align:center;margin-top:10px'>Auto Jelly Settings<br><div style='font-size:12px'>When you use a royal jelly or star jelly, more will be automatically used until the following requirements(or better) are met:</div></div>

    <div style="cursor:pointer;margin-top:10px;background-color:rgb(170,170,170);font-size:15px;border-radius:3px;" id="autoJellySettingsUntil">Roll until: Rare</div>
    <div style="cursor:pointer;margin-top:3px;background-color:rgb(170,170,170);font-size:15px;border-radius:3px;" id="autoJellySettingsGifted">Require gifted: No</div>

</div>

<div class='uiPage' style='position:absolute;margin:0px;z-index:-2;overflow-y:auto;overflow-x:hidden;width:200px;height:455px;background-color:rgba(195,195,195,0.9);margin-top:140px;display:none;padding:3px;font-size:13px;'></div>

<div id='howManyToFeed' style='position:fixed;width:300px;height:160px;z-index:1;left:50%;top:50%;transform:translate(-50%,-50%);display:none'>
    
    <div id='howManyMessage' style='position:fixed;margin-left:0px;margin-top:0px;width:300px;height:160px;background-color:rgb(30,70,255);border-radius:10px;color:rgb(255,255,255);font-size:17px;text-align:center'></div>
    
    <button id='feedThisAmount'>Feed:</button>
    <input id='feedAmount' value='1' type='number' min='1'>
    <button id='cancelFeeding'>Cancel</button>
    <button id='feedUntilGifted'>⚠️ Until Gifted</button>
    
</div>

<div id='dialogueBox' style='position:fixed;width:450px;height:200px;z-index:1;left:50%;top:80%;transform:translate(-50%,-50%);display:none;background-color:rgb(30,70,255);border-radius:10px;'>
    
    <div style='position:fixed;margin-left:5px;margin-top:5px;width:200px;height:30px;background-color:rgb(0,30,205);border-radius:10px;padding-left:10px;color:white;font-size:19px' id='NPCName'></div>
    
    <div style='position:fixed;margin-left:5px;margin-top:40px;width:430px;height:155px;background-color:rgb(0,30,205);border-radius:10px;padding-left:10px;color:white;font-size:19px' id='NPCDialogue'></div>
    
</div>

<div id='shopUI' style='position:fixed;z-index:1;width:100%;height:100%;display:none'>
    
    <div style='position:fixed;width:175px;height:300px;z-index:1;left:100%;top:45%;transform:translate(-100%,-50%);background-color:rgb(30,70,255);border-radius:10px;'>
        
        <div id='itemName' style='position:fixed;width:162px;height:30px;z-index:1;left:50%;top:7%;transform:translate(-50%,-50%);background-color:rgb(0,30,205);border-radius:10px;color:white;font-family:comic sans ms;font-size:18px;text-align:center'></div><div id='itemDesc' style='position:fixed;width:182px;height:280px;z-index:1;left:50%;top:56%;transform:translate(-50%,-50%) scale(0.9,0.9);background-color:rgb(0,30,205);border-radius:10px;color:white;font-family:comic sans ms;font-size:13px;text-align:center'></div>
        
    </div>
    
    <div style='position:fixed;width:117px;height:255px;z-index:1;left:0%;top:45%;transform:translate(0,-50%);background-color:rgb(30,70,255);border-radius:10px;'>
        
        <div id='itemCostSVG' style='position:fixed;width:110px;height:247px;z-index:1;left:50%;top:50%;transform:translate(-50%,-50%);background-color:rgb(0,30,205);border-radius:10px;color:white;font-family:comic sans ms;font-size:18px;'>
            
        </div>
        
    </div>
    
    <div style='position:fixed;border-radius:10px;background-color:rgb(0,30,205);width:200px;height:50px;left:50%;top:85%;transform:translate(-50%,-50%)'>
        
        <button id='purchaseButton' style='position:fixed;border-radius:10px;font-family:comic sans ms;background-color:rgb(0,200,0);text-align:center;width:100px;height:30px;left:50%;top:50%;transform:translate(-50%,-50%);color:white'>Purchase</button>
        
        <svg id='leftShopButton' style='position:fixed;width:30px;height:30px;left:13%;top:50%;transform:translate(-50%,-50%);background-color:rgb(30,90,255);border-radius:10px;cursor:pointer'>
            
            <path stroke='white' d='M20 10L10 15 L 20 20' stroke-width='3' fill='rgb(0,0,0,0)'></path>
            
        </svg>
        
        <svg id='rightShopButton' style='position:fixed;width:30px;height:30px;left:87%;top:50%;transform:translate(-50%,-50%);background-color:rgb(30,90,255);border-radius:10px;cursor:pointer'>
            
            <path stroke='white' d='M10 10L20 15 L 10 20' stroke-width='3' fill='rgb(0,0,0,0)'></path>
            
        </svg>
        
    </div>
    
</div>

<div id='blenderMenu' style='position:fixed;width:400px;height:300px;z-index:1;left:50%;top:50%;transform:translate(-50%,-50%);background-color:rgb(22, 54, 196);border-radius:4px;color:rgb(255,255,255);font-size:17px;text-align:center;display:none'>
    
    <button id='blenderX' style='border-radius:3px;margin-left:-365px;margin-top:3px;width:30px;height:30px;font-family:comic sans ms;padding-bottom:5px;'>X</button>
    
    <p id='blenderTitle' style='margin-top:-27px;font-size:18px'></p>
    
    <p id='blenderName' style='position:fixed;left:68%;top:12%;transform:translate(-50%,-50%);font-size:25px;width:500px'></p>
    
    <div id='blenderItem' style='position:fixed;left:22%;top:37%;transform:translate(-50%,-50%);width:115px;height:115px;'></div>
    
    <div id='blenderReq' style='position:fixed;left:50%;top:69%;transform:translate(-50%,-50%);width:400px;height:75px;'></div>
    
    <p id='blenderIndex' style='position:fixed;left:23%;top:84%;transform:translate(-50%,-50%);border-radius:5px;font-size:20px;width:500px'>1 / 20</p>
    
    <button id='blenderCraft' style='border-radius:3px;position:fixed;left:54%;top:91%;transform:translate(-50%,-50%);width:70px;height:30px;font-family:comic sans ms;padding-bottom:5px;color:white;font-size:17px;border:0px'>Craft:</button>
    
    <input style='background-color:rgb(19, 49, 133);width:60px;left:46%;top:69.5%' id='blenderCraftAmount' value='0' type='number' min='0'>
    
    <svg id='leftBlenderButton' style='position:fixed;width:50px;height:50px;left:7%;top:90.5%;transform:translate(-50%,-50%);border-radius:5px;cursor:pointer;'>
        
        <path stroke='white' d='M30 15L17 25 L 30 35' stroke-width='3' fill='rgb(0,0,0,0)'></path>
        
    </svg>
    
    <svg id='rightBlenderButton' style='position:fixed;width:50px;height:50px;left:93%;top:90.5%;transform:translate(-50%,-50%);border-radius:5px;cursor:pointer'>
        
        <path stroke='white' d='M30 15L17 25 L 30 35' stroke-width='3' fill='rgb(0,0,0,0)' transform=' translate(50,0) scale(-1,1)'></path>
        
    </svg>
    
    <button id='blenderEnd' style='border-radius:2px;position:fixed;left:20%;top:81%;transform:translate(-50%,-50%);width:120px;height:80px;font-family:comic sans ms;padding-bottom:5px;color:white;font-size:17px;border:2px solid black;'>End Crafting</button>
    
    <button id='blenderSpeed' style='border-radius:2px;position:fixed;left:80%;top:81%;transform:translate(-50%,-50%);width:120px;height:80px;font-family:comic sans ms;padding-bottom:5px;color:white;font-size:17px;border:2px solid black;'></button>

</div>

<div id='shrineMenu' style='position:fixed;width:400px;height:300px;z-index:1;left:50%;top:50%;transform:translate(-50%,-50%);background-color:rgb(22, 54, 196);border-radius:4px;color:rgb(255,255,255);font-size:17px;text-align:center;display:none'>
    
    <button id='shrineX' style='border-radius:3px;margin-left:-365px;margin-top:3px;width:30px;height:30px;font-family:comic sans ms;padding-bottom:5px;transition-duration:0s'>X</button>
    
    <p style='margin-left:15px;margin-top:-27px;font-size:22px'>What would you like to donate<br>to the Wind Shrine?</p>
    
    <p id='shrineItemName' style='margin-left:-180px;margin-top:29px;font-size:18px'>itemname</p>
    
    <div id='shrineItem' style='margin-left:170px;margin-top:-38px;'>a</div>
    
    <div id='shrineLeft' style='margin-top:-85px;margin-left:195px;font-size:25px;width:40px;border-radius:5px;'>◄</div>
    <div id='shrineRight' style='margin-top:-35px;margin-left:330px;font-size:25px;width:40px;border-radius:5px'>►</div>
    
    <p style='margin-left:0px;margin-top:21px;font-size:22px'>How many?</p>
    
    <input id='shrineAmount' value='1' type='number' min='1' style='margin-left:-60px;margin-top:-11px;width:100px;font-size:20px;height:19px;'>
    
    <div id='shrineDonate' style='margin-top:64px;margin-left:148px;font-size:22px;width:100px;border-radius:4px;padding-top:2px;padding-bottom:5px;'>Donate</div>
    
</div>

<div id='roboMenu' style='position:fixed;width:500px;height:500px;z-index:1;left:50%;top:50%;transform:translate(-50%,-50%);background-color:rgb(0,140,0);color:rgb(255,255,255);font-size:17px;text-align:center;border-radius:5px;display:none'>
    
    <div id='roboCogsAmount' style='position:fixed;left:22%;top:3%;transform:translate(-50%,-50%);font-size:20px;color:black;width:200px;text-align:left'></div>

    <div id='roboTitle' style='position:fixed;left:50%;top:7%;transform:translate(-50%,-50%);font-size:30px;'></div>

    <div id='roboStartRound' style='position:fixed;left:92%;top:7.5%;transform:translate(-50%,-50%);color:black;font-size:18px;background-color:rgb(46, 247, 93);padding:5px;border:2px solid black;border-radius:9px;cursor:pointer' onclick='window.roboStartRound()'>Start Round!</div>

    <div id='roboSkipBeePage' style='position:fixed;left:92%;top:7.5%;transform:translate(-50%,-50%);color:black;font-size:18px;background-color:rgb(46, 247, 93);padding:5px;border:2px solid black;border-radius:9px;cursor:pointer' onclick='window.roboSkipBeePage()'>Next</div>
    
    <div id='roboActiveBeesAmount' style='position:fixed;width:200px;height:400px;left:21%;top:54%;transform:translate(-50%,-50%);font-size:16px;'></div>
    
    <div id='roboActiveBees' style='position:fixed;width:200px;height:400px;left:21%;top:59%;transform:translate(-50%,-50%);font-size:29px;background-color:rgb(255,255,255,0.3);border-radius:4px;'></div>

    <div id='roboActiveUpgradesAmount' style='position:fixed;width:200px;height:400px;left:21%;top:54%;transform:translate(-50%,-50%);font-size:16px;'></div>
    
    <div id='roboActiveUpgrades' style='position:fixed;width:200px;height:400px;left:21%;top:59%;transform:translate(-50%,-50%);background-color:rgb(255,255,255,0.3);border-radius:4px;font-size:17px;text-align:left;color:black;overflow-y:auto;overflow-x:hidden'></div>
    
    <div id='roboBeeChoices' style='position:fixed;width:285px;height:400px;left:70.5%;top:59%;transform:translate(-50%,-50%);font-size:19px;'>
        
        <div id='roboBeeChoice1' style='cursor:pointer;position:fixed;background-color:rgb(255,255,255);border-radius:5px;width:255px;height:100px;left:50%;top:20%;transform:translate(-50%,-50%);color:black'></div>
        <div id='roboBeeChoice2' style='cursor:pointer;position:fixed;background-color:rgb(255,255,255);border-radius:5px;width:255px;height:100px;left:50%;top:50%;transform:translate(-50%,-50%);color:black'></div>
        <div id='roboBeeChoice3' style='cursor:pointer;position:fixed;background-color:rgb(255,255,255);border-radius:5px;width:255px;height:100px;left:50%;top:80%;transform:translate(-50%,-50%);color:black'></div>
        
    </div>

    <div id='roboQuestChoices' style='position:fixed;width:285px;height:400px;left:50%;top:59%;transform:translate(-50%,-50%);font-size:18px;font-family:comic sans ms'>
        
        <div id='roboQuestChoice1' style='cursor:pointer;position:fixed;background-color:rgb(255,255,255,0.4);border-radius:5px;width:235px;height:375px;left:7%;top:52%;transform:translate(-50%,-50%);color:black;font-size:16px;text-align:left;padding-left:5px;'></div>
        <div id='roboQuestChoice2' style='cursor:pointer;position:fixed;background-color:rgb(255,255,255,0.4);border-radius:5px;width:235px;height:375px;left:93%;top:52%;transform:translate(-50%,-50%);color:black;font-size:16px;text-align:left;padding-left:5px;'></div>
        
    </div>

    <div id='roboUpgradeChoices' style='position:fixed;width:285px;height:400px;left:70.5%;top:59%;transform:translate(-50%,-50%);font-size:17px;text-align:left;'>
        
        <div id='roboUpgradeChoice1' style='cursor:pointer;position:fixed;background-color:rgb(243, 225, 165);border-radius:5px;width:285px;height:125px;left:50%;top:17%;transform:translate(-50%,-50%);color:black'></div>
        <div id='roboUpgradeChoice2' style='cursor:pointer;position:fixed;background-color:rgb(243, 225, 165);border-radius:5px;width:285px;height:125px;left:50%;top:50%;transform:translate(-50%,-50%);color:black'></div>
        <div id='roboUpgradeChoice3' style='cursor:pointer;position:fixed;background-color:rgb(243, 225, 165);border-radius:5px;width:285px;height:125px;left:50%;top:83%;transform:translate(-50%,-50%);color:black'></div>
        
    </div>
    
</div>

<div id='roboQuestMenu' style='position:fixed;width:200px;height:250px;z-index:-3;left:-5px;top:50%;transform:translate(0%,-50%);background-color:rgb(0,140,0);color:rgb(255,255,255);font-size:21px;text-align:center;border-radius:9px;display:none;border:4px solid rgb(100,100,100)'>
        
    <div id='roboQuestSeparator'></div>

    <div id='endRoboChallenge' style='position:absolute;left:50%;top:100%;width:150px;border-radius:10px;border:3px solid black;color:black;background-color:rgb(255,30,30);cursor:pointer;font-size:20px;transform:translate(-50%,-50%);padding:3px' onclick='window.endRoboChallenge()'>End Challenge</div>

</div>

<div id='actionWarning' style='position:fixed;margin-left:50%;margin-top:5px;width:360px;height:60px;z-index:2;transform:translate(-50%,0px);display:none'>
    
    <div style='position:fixed;margin-left:60px;margin-top:5.5px;width:300px;height:50px;background-color:rgb(30,70,255,0.8);border-radius:3px;padding:0px;font-size:18px;color:white;text-align:center;padding-top:0px' id='actionNameBox'><div id='actionName' style='position:absolute;left:50%;top:50%;transform:translate(-50%,-50%) translate(0,-2px);width:300px;'></div></div>

    <div style='position:fixed;margin-left:0px;margin-top:0px;width:45px;height:55px;background-color:rgb(200,200,200);border-radius:3px;font-size:40px;color:white;padding-left:15px;padding-top:0px;border:3px solid rgb(50,50,50)' id='actionEButton'>E</div>

    <div style='position:fixed;margin-left:0px;margin-top:0px;width:360px;height:50px;padding-left:0px;padding-top:10px' id='actionHoverDarken'></div>
    
</div>

<div id='passiveActivationPopup' style='position:fixed;z-index:1'></div>

<div id='meshCreator' style='position:fixed;background-color:rgb(200,200,200,0.4);left:500px;top:0px;width:300px;height:450px;'>
    
    <button id='runMeshStr' style='margin:3px'>Run</button>
    <span style='margin:3px;display:block'></span>
    <textarea id='meshStr' type='text' cols='45' rows='31'style='font-size:11px'></textarea>
    
</div>

<div class='hotbarSlot' style='position:fixed;top:93%;left:25%;width:36px;height:36px;z-index:-1;transform:translate(-50%,-50%);border-radius:10px;border:6px solid rgb(200,200,200)'></div>
<div class='hotbarSlot' style='position:fixed;top:93%;left:35%;width:36px;height:36px;z-index:-1;transform:translate(-50%,-50%);border-radius:10px;border:6px solid rgb(200,200,200)'></div>
<div class='hotbarSlot' style='position:fixed;top:93%;left:45%;width:36px;height:36px;z-index:-1;transform:translate(-50%,-50%);border-radius:10px;border:6px solid rgb(200,200,200)'></div>
<div class='hotbarSlot' style='position:fixed;top:93%;left:55%;width:36px;height:36px;z-index:-1;transform:translate(-50%,-50%);border-radius:10px;border:6px solid rgb(200,200,200)'></div>
<div class='hotbarSlot' style='position:fixed;top:93%;left:65%;width:36px;height:36px;z-index:-1;transform:translate(-50%,-50%);border-radius:10px;border:6px solid rgb(200,200,200)'></div>
<div class='hotbarSlot' style='position:fixed;top:93%;left:75%;width:36px;height:36px;z-index:-1;transform:translate(-50%,-50%);border-radius:10px;border:6px solid rgb(200,200,200)'></div>
<div class='autoHotbarButton' style='position:fixed;top:98.5%;left:25%;width:39px;height:11px;z-index:-1;transform:translate(-50%,-50%);border-radius:10px;padding-top:1px;font-family:trebuchet ms;font-size:7px;text-align:center'></div>
<div class='autoHotbarButton' style='position:fixed;top:98.5%;left:35%;width:39px;height:11px;z-index:-1;transform:translate(-50%,-50%);border-radius:10px;padding-top:1px;font-family:trebuchet ms;font-size:7px;text-align:center'></div>
<div class='autoHotbarButton' style='position:fixed;top:98.5%;left:45%;width:39px;height:11px;z-index:-1;transform:translate(-50%,-50%);border-radius:10px;padding-top:1px;font-family:trebuchet ms;font-size:7px;text-align:center'></div>
<div class='autoHotbarButton' style='position:fixed;top:98.5%;left:55%;width:39px;height:11px;z-index:-1;transform:translate(-50%,-50%);border-radius:10px;padding-top:1px;font-family:trebuchet ms;font-size:7px;text-align:center'></div>
<div class='autoHotbarButton' style='position:fixed;top:98.5%;left:65%;width:39px;height:11px;z-index:-1;transform:translate(-50%,-50%);border-radius:10px;padding-top:1px;font-family:trebuchet ms;font-size:7px;text-align:center'></div>
<div class='autoHotbarButton' style='position:fixed;top:98.5%;left:75%;width:39px;height:11px;z-index:-1;transform:translate(-50%,-50%);border-radius:10px;padding-top:1px;font-family:trebuchet ms;font-size:7px;text-align:center'></div>

<div id='amuletUI' style='position:fixed;left:50%;top:50%;width:250px;height:350px;z-index:2;transform:translate(-50%,-50%) scale(1.25,1.25);background-color:rgb(255,255,100);border-radius:4px;display:none'>
    
    <div style='position:fixed;margin-left:50%;margin-top:0px;width:250px;height:35px;z-index:2;transform:translate(-50%,0px);background-color:rgb(30,200,70);border-bottom:2px solid rgb(0,100,0);text-align:center;font-size:23px;font-family:trebuchet ms;padding-top:5px;border-radius:4px' id='amuletType'></div>
    
    <div style='position:fixed;margin-left:0px;margin-top:48px;width:125px;height:22px;z-index:2;background-color:rgb(255,20,20,0.5);text-align:center;font-size:17px;font-family:trebuchet ms;padding-top:2px'>Old:</div>
    <div style='position:fixed;margin-left:50%;margin-top:48px;width:125px;height:22px;z-index:2;background-color:rgb(0,190,50,0.5);text-align:center;font-size:17px;font-family:trebuchet ms;padding-top:2px'>New:</div>
    
    <div style='position:fixed;margin-left:0px;margin-top:75px;width:120px;height:220px;z-index:2;font-size:10.5px;font-family:trebuchet ms;padding-left:5px' id='oldAmuletStats'></div>
    <div style='position:fixed;margin-left:50%;margin-top:75px;width:120px;height:220px;z-index:2;font-size:10.5px;font-family:trebuchet ms;padding-left:5px' id='newAmuletStats'></div>
    
    <div style='position:fixed;margin-left:4%;margin-top:310px;width:100px;height:28px;z-index:2;background-color:rgb(200,0,0);text-align:center;font-size:19px;font-family:trebuchet ms;padding-top:4px;color:white;border-radius:2px' id='keepAmulet'>Keep Old</div>
    <div style='position:fixed;margin-left:56%;margin-top:310px;width:100px;height:28px;z-index:2;background-color:rgb(0,130,20);text-align:center;font-size:19px;font-family:trebuchet ms;padding-top:4px;color:white;border-radius:2px' id='replaceAmulet'>Replace</div>
</div>

<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gl-matrix/2.8.1/gl-matrix-min.js"></script>
<script src='https://cdnjs.cloudflare.com/ajax/libs/cannon.js/0.6.2/cannon.min.js'></script>
<script src="https://cdn.jsdelivr.net/npm/canvg/dist/browser/canvg.min.js"></script>

<script src="https://cdn.jsdelivr.net/gh/Dddatt/bss@v2.0.13/dialogue.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/Dddatt/bss@v2.0.13/textures.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/Dddatt/bss@v2.0.13/playerGear.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/Dddatt/bss@v2.0.13/MathHelper.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/Dddatt/bss@v2.0.13/GLSLShaders.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/Dddatt/bss@v2.0.13/mapMesh.min.js"></script>
<script src="https://cdn.jsdelivr.net/gh/Dddatt/bss@v2.0.13/thumbnail.min.js"></script>

<script src="https://cdn.jsdelivr.net/gh/Dddatt/bss@v2.0.13/index.min.js"></script>

<!--<script>-->

</body>
</html>
