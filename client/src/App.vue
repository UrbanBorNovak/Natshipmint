<template>
  <div id="app">
    <div class="container">
      <h1 id="title">Ordinal Shipyard <sub>by urbannoki</sub></h1>
      <p>Input your ordinal ID, then press "Observe Vessel Plans"</p>
      <div id="helioCheckoutContainer"></div>

      <label for="blockId">Enter Inscription ID:</label>
      <input type="text" id="blockId" v-model="blockId" required>
      <button @click="fetchAndVisualize">Observe Vessel Plans</button>
    </div>
    <div id="blockData" class="container"></div>
    <canvas id="visualizationCanvas" width="800" height="400"></canvas>
    <div id="blockData"></div>

    <div class="ships">
      <Ship v-for="ship in ships" :key="ship.name" :ship="ship" />
    </div>

    <div id="natcatviewer"></div>
    <input id="blk" type="number" style="display:none" />
  </div>
</template>

<script>
import Ship from './components/Ship.vue';

export default {
  name: 'App',
  data() {
    return {
      blockId: '',
      ships: [
        { name: 'Coastal Tanker', size: '29m', length: '205m' },
        { name: 'Aframax', size: '34m', length: '255m' },
        { name: 'Suez-Max', size: '45m', length: '285m' },
        { name: 'VLCC', size: '55m', length: '330m' },
        { name: 'ULCC', size: '63m', length: '415m' }
      ]
    };
  },
  methods: {
    async fetchAndVisualize() {
      const response = await fetch(`https://api.hiro.so/ordinals/v1/inscriptions/${this.blockId}`);
      const data = await response.json();
      this.visualizeBlockData(data);
    },
    visualizeBlockData(data) {
      const blockNumber = data.blk;
      this.generateHtmlBasedOnBlockNumber(blockNumber);
    },
    generateHtmlBasedOnBlockNumber(blockNumber) {
      const lookDir = this.getLook(blockNumber);
      const lookHtml = this.generateLookSvg(lookDir);
      let catnip = this.c420(blockNumber) ? this.displaycatnip() : "";
      let cig = this.c4a0(blockNumber) ? this.displaycig() : "";
      let earring = this.c0(blockNumber) ? this.displayEarring() : "";
      let earringR = this.c00(blockNumber) ? this.displayEarringR() : "";
      let alienEarring = this.c000(blockNumber) ? this.displayAlienEarring() : "";
      let alienEarringR = this.c0000(blockNumber) ? this.displayAlienEarringR() : "";
      let alienTiara = this.c00000(blockNumber) ? this.displayAlienTiara() : "";
      let fly = this.c11(blockNumber) ? this.displayFly() : "";
      let flysEarring = this.c111(blockNumber) ? this.displayFlysEarring() : "";
      let flysAlienEarring = this.c1111(blockNumber) ? this.displayFlysAlienEarring() : "";
      let flysLaserEyes = this.c11111(blockNumber) ? this.displayFlysLaserEyes() : "";
      let bowR = this.c8a8(blockNumber) ? this.displayBowR() : "";
      let bowL = this.c88(blockNumber) ? this.displayBowL() : "";
      let bowTail = this.c888(blockNumber) ? this.displayBowTail() : "";
      let bowAlienBowTie = this.c8888(blockNumber) ? this.displayAlienBowTie() : "";
      let bowDoubleAlienBowTie = this.c88888(blockNumber) ? this.displayDoubleAlienBowTie() : "";
      let alienDiamond = this.cp6(blockNumber) ? this.displayAlienDiamond() : "";
      let yarn = this.cs5(blockNumber) ? this.displayYarn() : "";
      let laserPointer = this.cs6(blockNumber) ? this.displayLaserPointer() : "";
      let trout = this.c9a9(blockNumber) ? this.displayTrout() : "";
      let salmon = this.c99(blockNumber) ? this.displaySalmon() : "";
      let alienFish = this.c999(blockNumber) ? this.displayAlienFish() : "";
      let giantAlienFish = this.c9999(blockNumber) ? this.displayGiantAlienFish() : "";
      let bloodDrips = this.cf3(blockNumber) ? this.displayBloodDrips() : "";
      let browPiercing = this.cf4(blockNumber) ? this.displayBrowPiercing() : "";
      let halo = this.cf5(blockNumber) ? this.displayHalo() : "";
      let hammer = this.cf6(blockNumber) ? this.displayHammer() : "";
      let vial = this.cf7(blockNumber) ? this.displayVial() : "";
      let mouse = this.m11(blockNumber) ? this.displayMouse() : "";
      let alienMouse = this.m888(blockNumber) ? this.displayAlienMouse() : "";
      let nightVision = this.ce7(blockNumber) ? this.displayNightVision() : "";
      let rainbowCollar = this.m12(blockNumber) ? this.displayRainbowCollar() : "";
      let pearls = this.m13(blockNumber) ? this.displayPearls() : "";
      let orangeCollar = this.m14(blockNumber) ? this.displayOrangeCollar() : "";
      let headBand = this.m15(blockNumber) ? this.displayHeadBand() : "";
      let sunHat = this.m16(blockNumber) ? this.displaySunHat() : "";
      let spikeCollar = this.m69(blockNumber) ? this.displaySpikeCollar() : "";
      const containsSquare = this.containsFourDigitSquare(blockNumber);
      const eyeDirection = this.getLaserEyeRange(parseInt(blockNumber.toString().slice(-4)));
      const laserEyes = containsSquare ? this.displayLaserEyes(eyeDirection) : "";
      const svgsForDigits = this.generateSvgForDigits(blockNumber);
      const svgsForDigits2 = this.generateSvgForDigits2(blockNumber);

      //background
      const background = `<div style="position: absolute; top: 0px; left: 0px;">
      <svg width="425" height="425" viewBox="0 0 425 425" fill="none" xmlns="http://www.w3.org/2000/svg">
          <!-- Background -->
          <rect width="425" height="425" fill="#201F27"/>
          <!-- Water Gradient -->
          <defs>
              <linearGradient id="waterGradient" x1="0%" y1="0%" x2="100%" y2="100%">
                  <stop offset="0%" style="stop-color:#00539C;stop-opacity:1" />
                  <stop offset="50%" style="stop-color:#0083B0;stop-opacity:1" />
                  <stop offset="100%" style="stop-color:#00A2D2;stop-opacity:1" />
              </linearGradient>
          </defs>
          <!-- Water Texture -->
          <rect width="425" height="425" fill="url(#waterGradient)"/>
      </svg>
  </div>`;

      //mouth
      const mouth = `<div style="position: absolute; top: 150px; left: 20px;">
      <svg width="50" height="110" viewBox="0 0 50 70" fill="none" xmlns="http://www.w3.org/2000/svg">
          <!-- Flag -->
          <path d="M0 0L80 0L100 20L80 40L0 40V0Z" fill="#B22222"/>
          <!-- Shade -->
          <path d="M0 0L40 0L50 10L40 20L0 20V0Z" fill="#7B1113" fill-opacity="0.4"/>
          <!-- Pole -->
          <rect x="45" y="0" width="5" height="50" fill="#808080"/>
          <!-- Metal Texture -->
          <rect x="45" y="0" width="5" height="50" fill="url(#metalGradient)"/>
          <!-- Metal Gradient -->
          <defs>
              <linearGradient id="metalGradient" x1="0%" y1="0%" x2="100%" y2="100%">
                  <stop offset="0%" style="stop-color:#B0B0B0;stop-opacity:1" />
                  <stop offset="50%" style="stop-color:#808080;stop-opacity:1" />
                  <stop offset="100%" style="stop-color:#B0B0B0;stop-opacity:1" />
              </linearGradient>
          </defs>
      </svg>
  </div>`

      //eyes
      const eyes = `<div style="position: absolute; top: 172px; left: 85px;">
      <svg width="80" height="48" viewBox="0 0 80 48" fill="none" xmlns="http://www.w3.org/2000/svg">
          <defs>
              <linearGradient id="shadowGradient" x1="0%" y1="0%" x2="100%" y2="100%">
                  <stop offset="0%" style="stop-color:rgba(0,0,0,0.3);stop-opacity:1" />
                  <stop offset="100%" style="stop-color:rgba(0,0,0,0);stop-opacity:0" />
              </linearGradient>
              <linearGradient id="highlightGradient" x1="0%" y1="0%" x2="100%" y2="100%">
                  <stop offset="0%" style="stop-color:rgba(255,255,255,0.6);stop-opacity:1" />
                  <stop offset="100%" style="stop-color:rgba(255,255,255,0);stop-opacity:0" />
              </linearGradient>
          </defs>

          <g>
              <!-- Side -->
              <polygon points="0,0 8,8 8,32 0,40" fill="#B2B2B2" />
              <!-- Top -->
              <polygon points="0,0 56,0 64,8 8,8" fill="#8C8C8C" />
              <!-- Front -->
              <polygon points="8,8 64,8 64,32 8,32" fill="#666666" />
              <!-- Shadows -->
              <polygon points="0,0 8,8 64,8 56,0" fill="#999999" fill-opacity="0.4" />
              <rect x="8" y="8" width="56" height="24" fill="#777777" fill-opacity="0.4" />
              <!-- Highlight -->
              <rect x="8" y="8" width="56" height="24" fill="url(#highlightGradient)" />
          </g>
      </svg>
  </div>`

      //teeth
      const teeth = `
          <div style="position: absolute; top: 172px; left: 262px;">
      <svg width="80" height="26" viewBox="0 0 80 26" fill="none" xmlns="http://www.w3.org/2000/svg">
          <defs>
              <linearGradient id="shadowGradient" x1="0%" y1="0%" x2="100%" y2="100%">
                  <stop offset="0%" style="stop-color:rgba(0,0,0,0.3);stop-opacity:1" />
                  <stop offset="100%" style="stop-color:rgba(0,0,0,0);stop-opacity:0" />
              </linearGradient>
              <linearGradient id="highlightGradient" x1="0%" y1="0%" x2="100%" y2="100%">
                  <stop offset="0%" style="stop-color:rgba(255,255,255,0.6);stop-opacity:1" />
                  <stop offset="100%" style="stop-color:rgba(255,255,255,0);stop-opacity:0" />
              </linearGradient>
          </defs>

          <g>
              <!-- Side -->
              <path d="M0 0L8 5V21L0 26V0Z" fill="#B2B2B2" />
              <!-- Top -->
              <path d="M0 0L56 0V5L8 5L0 0Z" fill="#8C8C8C" />
              <!-- Front -->
              <path d="M8 5L64 5V21L8 26V5Z" fill="#666666" />
              <!-- Shadows -->
              <polygon points="0,0 8,5 64,5 56,0" fill="#999999" fill-opacity="0.7" />
              <rect x="8" y="5" width="56" height="16" fill="#777777" fill-opacity="0.4" />
              <!-- Highlight -->
              <rect x="8" y="5" width="56" height="16" fill="url(#highlightGradient)" />
          </g>
      </svg>
  </div>`

      //construction
      const htmlContent = background + earring + alienEarring + earringR + alienEarringR + eyes + nightVision + lookHtml + teeth + svgsForDigits2 + mouth + svgsForDigits + cig + catnip + fly + flysEarring + flysAlienEarring + headBand + sunHat + bowL + bowR + bowTail + rainbowCollar + orangeCollar + spikeCollar + pearls + bowAlienBowTie + bowDoubleAlienBowTie + trout + salmon + alienFish + giantAlienFish + yarn + laserPointer + halo + bloodDrips + hammer + vial + mouse + browPiercing + alienMouse + alienDiamond + alienTiara + flysLaserEyes + laserEyes
      document.getElementById('natcatviewer').innerHTML = htmlContent
    },
    getLook(number) {
      const numberStr = number.toString();
      const lastFourDigits = parseInt(numberStr.substring(numberStr.length - 4));

      if (lastFourDigits < 4800) {
        return "look_right";
      } else if (lastFourDigits >= 4800 && lastFourDigits <= 5200) {
        return "look_crossed";
      } else {
        return "look_left";
      }
    },
    generateLookSvg(lookDir) {
      let lookHtml = "";
      if (lookDir === "look_left") {
        lookHtml = `<div style="position: absolute; top: 150px; left: 156px;">
                        <svg width="158" height="43" viewBox="0 0 158 43" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <rect x="37" y="33" width="37" height="42" transform="rotate(-180 37 38)" fill="#070609"/>
                        <rect x="158" y="33" width="37" height="33" transform="rotate(-180 158 38)" fill="#070609"/>
                        </svg>
                    </div>`;
      } else if (lookDir === "look_right") {
        lookHtml = `<div style="position: absolute; top: 200px; left: 123px;">
                        <svg width="195" height="35" viewBox="0 0 258 89" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <rect x="-10" y="32" width="89" height="60" transform="rotate(-180 37 38)" fill="#070609"/>
                        <rect x="107.5" y="32" width="69" height="60" transform="rotate(-180 157.5 38)" fill="#070609"/>
                        </svg>
                    </div>`;
      } else if (lookDir === "look_crossed") {
        lookHtml = `<div style="position: absolute; top: 155px; left: 240px;">
                        <svg width="195" height="38" viewBox="0 0 195 38" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <rect x="295.5" y="38" width="36.5" height="38" transform="rotate(-180 194.5 38)" fill="#070609"/>
                        <rect x="37" y="38" width="37" height="38" transform="rotate(-180 37 38)" fill="#070609"/>
                        </svg>
                    </div>`;
      }
      return lookHtml;
    },
    generateSvgForDigits(blockNumber) {
      const originalString = blockNumber.toString();
      const digits = originalString.padStart(7, '0').split('').map(Number).reverse();
      let svgs = [];

      // Left ear
      if (originalString.length >= 3) {
        const colorForThirdDigit = this.colorMap2[digits[2]] || "transparent";
        svgs.push(`
          <div style="position: absolute; top: 162px; left: 320px;">
            <svg width="50" height="55" viewBox="0 0 100 150" fill="none" xmlns="http://www.w3.org/2000/svg">
                <!-- Define linear gradients for shadow and highlight -->
                <defs>
                    <linearGradient id="shadowGradient" x1="0%" y1="0%" x2="100%" y2="100%">
                        <stop offset="0%" stop-color="rgba(0,0,0,0.3)"/>
                        <stop offset="100%" stop-color="rgba(0,0,0,0)"/>
                    </linearGradient>
                    <linearGradient id="highlightGradient" x1="0%" y1="0%" x2="100%" y2="100%">
                        <stop offset="0%" stop-color="rgba(255,255,255,0.6)"/>
                        <stop offset="100%" stop-color="rgba(255,255,255,0)"/>
                    </linearGradient>
                </defs>

                <!-- Ship's Prow -->
                <path d="M0 150L50 0L90 150H0Z" fill="${colorForThirdDigit}"/>
                
                <!-- Add shadow -->
                <path d="M0 150L50 0L80 150H0Z" fill="url(#shadowGradient)"/>

                <!-- Add highlight -->
                <path d="M50 0L90 150H50L50 0Z" fill="url(#highlightGradient)"/>

                <!-- Radar Antenna -->
                <rect x="40" y="50" width="10" height="40" fill="#FF4500" rx="3" ry="3"/>

                <!-- Add shadow to antenna -->
                <rect x="40" y="50" width="10" height="40" fill="url(#shadowGradient)" rx="3" ry="3"/>

                <!-- Add highlight to antenna -->
                <rect x="40" y="50" width="10" height="40" fill="url(#highlightGradient)" rx="3" ry="3"/>

                <!-- Radar Dish -->
                <circle cx="45" cy="40" r="12" fill="${colorForThirdDigit}"/>

                <!-- Add shading to dish -->
                <circle cx="45" cy="40" r="12" fill="url(#shadowGradient)"/>

                <!-- Add highlight to dish -->
                <circle cx="45" cy="40" r="12" fill="url(#highlightGradient)"/>
            </svg>
          </div>
        `);
      }

      // Right ear
      if (originalString.length >= 4) {
        const colorForFourthDigit = this.colorMap2[digits[3]] || "transparent";
        svgs.push(`
          <div style="position: absolute; top: 77px; left: 82px;">
            <svg width="70" height="70" viewBox="0 0 70 70" fill="none" xmlns="http://www.w3.org/2000/svg">
                <!-- Define linear gradients for shadow and highlight -->
                <defs>
                    <linearGradient id="radarBodyGradient" x1="0%" y1="0%" x2="100%" y2="100%">
                        <stop offset="0%" stop-color="${colorForFourthDigit}" stop-opacity="0.2"/>
                        <stop offset="100%" stop-color="${colorForFourthDigit}" stop-opacity="0.6"/>
                    </linearGradient>
                    <linearGradient id="radarHighlightGradient" x1="0%" y1="0%" x2="100%" y2="100%">
                        <stop offset="0%" stop-color="rgba(255, 255, 255, 0.6)"/>
                        <stop offset="100%" stop-color="rgba(255, 255, 255, 0)"/>
                    </linearGradient>
                    <linearGradient id="radarShadowGradient" x1="0%" y1="0%" x2="100%" y2="100%">
                        <stop offset="0%" stop-color="rgba(0, 0, 0, 0.3)"/>
                        <stop offset="100%" stop-color="rgba(0, 0, 0, 0)"/>
                    </linearGradient>
                </defs>

                <!-- Radar Body -->
                <circle cx="35" cy="35" r="28" fill="url(#radarBodyGradient)"/>

                <!-- Radar Highlight -->
                <circle cx="35" cy="35" r="28" fill="url(#radarHighlightGradient)"/>

                <!-- Radar Shadow -->
                <circle cx="35" cy="35" r="28" fill="url(#radarShadowGradient)"/>

                <!-- Radar Lines -->
                <circle cx="35" cy="35" r="21" stroke="${colorForFourthDigit}" stroke-width="2" fill="none"/>
                <circle cx="35" cy="35" r="14" stroke="${colorForFourthDigit}" stroke-width="2" fill="none"/>
                <circle cx="35" cy="35" r="7" stroke="${colorForFourthDigit}" stroke-width="2" fill="none"/>

                <!-- Radar Sweep -->
                <path d="M35 7 A28 28 0 0 1 63 35" stroke="${colorForFourthDigit}" stroke-width="2" fill="none"/>

                <!-- Base -->
                <rect x="30" y="60" width="10" height="10" fill="${colorForFourthDigit}"/>
            </svg>
          </div>
        `);
      }

      // Neck
      if (originalString.length >= 2) {
        const colorForSecondDigit = this.colorMap2[digits[1]] || "transparent";
        svgs.push(`
          <div style="position: absolute; top: 72px; left: 117px;">
            <svg width="178" height="180" viewBox="0 0 178 180" fill="none" xmlns="http://www.w3.org/2000/svg">
                <defs>
                    <linearGradient id="shadowGradient" x1="0%" y1="0%" x2="0%" y2="100%">
                        <stop offset="0%" style="stop-color:rgba(0,0,0,0.3);stop-opacity:1" />
                        <stop offset="100%" style="stop-color:rgba(0,0,0,0);stop-opacity:0" />
                    </linearGradient>
                    <linearGradient id="midtoneGradient" x1="0%" y1="0%" x2="0%" y2="100%">
                        <stop offset="0%" style="stop-color:#B2B2B2;stop-opacity:1" />
                        <stop offset="100%" style="stop-color:#B2B2B2;stop-opacity:0" />
                    </linearGradient>
                    <linearGradient id="highlightGradient" x1="0%" y1="0%" x2="0%" y2="100%">
                        <stop offset="0%" style="stop-color:rgba(255,255,255,0.6);stop-opacity:1" />
                        <stop offset="100%" style="stop-color:rgba(255,255,255,0);stop-opacity:0" />
                    </linearGradient>
                </defs>
                
                <g>
                    <!-- Base of the superstructure -->
                    <rect x="34" y="100" width="110" height="20" fill="${colorForSecondDigit}" />
                    <rect x="34" y="100" width="110" height="20" fill="url(#midtoneGradient)" />
                    <path d="M34 100L44 120L144 120L134 100H34Z" fill="#777777" fill-opacity="0.5" />
                    
                    <!-- Cabin -->
                    <rect x="54" y="40" width="70" height="60" fill="${colorForSecondDigit}" />
                    <rect x="54" y="40" width="70" height="60" fill="url(#midtoneGradient)" />
                    <path d="M54 40L64 60L124 60L114 40H54Z" fill="#9C9C9C" fill-opacity="0.6" />
                    
                    <!-- Highlight -->
                    <rect x="54" y="40" width="70" height="20" fill="url(#highlightGradient)" />
                    
                    <!-- Top of the superstructure -->
                    <rect x="54" y="30" width="70" height="10" fill="${colorForSecondDigit}" />
                    <rect x="54" y="30" width="70" height="10" fill="url(#shadowGradient)" />
                    <path d="M54 30L64 40L124 40L114 30H54Z" fill="#888888" fill-opacity="0.7" />
                </g>
            </svg>
          </div>`);
      }

      // Hull
      if (originalString.length >= 1) {
        const colorForFirstDigit = this.colorMap[digits[0]] || "transparent";
        svgs.push(`
          <div style="position: absolute; top: 88px; left: 45px;">
            <svg width="325" height="309" viewBox="0 0 325 309" fill="none" xmlns="http://www.w3.org/2000/svg">
                <!-- Define dynamic hull color -->
                <linearGradient id="hullGradient" x1="0%" y1="0%" x2="100%" y2="100%">
                    <stop offset="0%" stop-color="${colorForFirstDigit}" stop-opacity="0.6"/>
                    <stop offset="100%" stop-color="${colorForFirstDigit}" stop-opacity="1"/>
                </linearGradient>

                <!-- Hull shape -->
                <path d="M162.5,309 C120.5,285 78.5,250 50,115 C50,100 275,100 275,115 C247.5,250 205.5,285 162.5,309Z" fill="url(#hullGradient)" stroke="#000" stroke-width="2"/>
                
                <!-- Front Highlight -->
                <linearGradient id="highlightGradientFront" x1="0%" y1="0%" x2="100%" y2="100%">
                    <stop offset="0%" stop-color="white" stop-opacity="0.5"/>
                    <stop offset="100%" stop-color="white" stop-opacity="0"/>
                </linearGradient>
                <path d="M162.5,309 C120.5,285 78.5,250 50,115 C50,100 162.5,100 162.5,115 C205.5,250 205.5,285 162.5,309Z" fill="url(#highlightGradientFront)"/>

                <!-- Back Shadow -->
                <linearGradient id="shadowGradientBack" x1="0%" y1="0%" x2="100%" y2="100%">
                    <stop offset="0%" stop-color="black" stop-opacity="0"/>
                    <stop offset="100%" stop-color="black" stop-opacity="0.5"/>
                </linearGradient>
                <path d="M162.5,309 C205.5,285 247.5,250 275,115 C275,100 162.5,100 162.5,115 C205.5,250 205.5,285 162.5,309Z" fill="url(#shadowGradientBack)"/>
            </svg>
          </div>
        `);
      }

      return svgs.join('');
    },
    generateSvgForDigits2(blockNumber) {
      const digits = blockNumber.toString().split('').map(Number).reverse();
      let svgs = [];

      // Generate colors based on the digits
      digits.forEach((digit, index) => {
        const color = this.colorMap[digit] || "transparent";
        svgs.push(`<rect x="${index * 50}" y="0" width="50" height="50" fill="${color}" />`);
      });

      return svgs.join('');
    },
    containsFourDigitSquare(number) {
      const originalString = number.toString();
      const digits = originalString.padStart(4, '0').split('').map(Number);
      const squareNumbers = [0, 1, 4, 9];
      return digits.every(digit => squareNumbers.includes(digit));
    },
    getLaserEyeRange(number) {
      return number <= 5000 ? "short" : number <= 7500 ? "medium" : "long";
    },
    displayLaserEyes(range) {
      const laserPaths = {
        short: `<svg width="256" height="256" viewBox="0 0 256 256" fill="none" xmlns="http://www.w3.org/2000/svg">
            <line x1="20" y1="80" x2="120" y2="200" stroke="red" stroke-width="4"/>
            <line x1="236" y1="80" x2="136" y2="200" stroke="red" stroke-width="4"/>
          </svg>`,
        medium: `<svg width="256" height="256" viewBox="0 0 256 256" fill="none" xmlns="http://www.w3.org/2000/svg">
            <line x1="20" y1="60" x2="140" y2="220" stroke="red" stroke-width="6"/>
            <line x1="236" y1="60" x2="116" y2="220" stroke="red" stroke-width="6"/>
          </svg>`,
        long: `<svg width="256" height="256" viewBox="0 0 256 256" fill="none" xmlns="http://www.w3.org/2000/svg">
            <line x1="20" y1="40" x2="160" y2="240" stroke="red" stroke-width="8"/>
            <line x1="236" y1="40" x2="96" y2="240" stroke="red" stroke-width="8"/>
          </svg>`
      };
      return laserPaths[range] || "";
    },
    c0: (n) => n % 16 === 0,
    c00: (n) => n % 256 === 0,
    c000: (n) => n % 4096 === 0,
    c0000: (n) => n % 65536 === 0,
    c00000: (n) => n % 1048576 === 0,
    c4a0: (n) => n % 2 === 0,
    c420: (n) => n % 3 === 0,
    c8a8: (n) => n % 4 === 0,
    c88: (n) => n % 5 === 0,
    c888: (n) => n % 6 === 0,
    c8888: (n) => n % 7 === 0,
    c88888: (n) => n % 8 === 0,
    cp6: (n) => n % 9 === 0,
    cs5: (n) => n % 10 === 0,
    cs6: (n) => n % 11 === 0,
    c9a9: (n) => n % 12 === 0,
    c99: (n) => n % 13 === 0,
    c999: (n) => n % 14 === 0,
    c9999: (n) => n % 15 === 0,
    cf3: (n) => n % 17 === 0,
    cf4: (n) => n % 18 === 0,
    cf5: (n) => n % 19 === 0,
    cf6: (n) => n % 20 === 0,
    cf7: (n) => n % 21 === 0,
    m11: (n) => n % 22 === 0,
    m888: (n) => n % 23 === 0,
    ce7: (n) => n % 24 === 0,
    m12: (n) => n % 25 === 0,
    m13: (n) => n % 26 === 0,
    m14: (n) => n % 27 === 0,
    m15: (n) => n % 28 === 0,
    m16: (n) => n % 29 === 0,
    m69: (n) => n % 30 === 0,
    colorMap: {
      0: "#FF5733", 1: "#33FF57", 2: "#3357FF", 3: "#FF33A8", 4: "#A833FF", 5: "#33FFF2",
      6: "#FFD733", 7: "#FF3333", 8: "#33FF33", 9: "#5733FF"
    },
    colorMap2: {
      0: "#8B4513", 1: "#9ACD32", 2: "#5F9EA0", 3: "#D2691E", 4: "#DC143C", 5: "#00CED1",
      6: "#B8860B", 7: "#32CD32", 8: "#6A5ACD", 9: "#FF6347"
    }
  }
};
</script>

<style scoped>
.container {
  text-align: center;
  margin: 20px auto;
}

.ships {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
}

input, button {
  padding: 10px;
  margin: 5px;
}
</style>
