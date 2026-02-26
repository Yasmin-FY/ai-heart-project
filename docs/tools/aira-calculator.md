# AIRA Health Score Calculator

AIRA Health is a risk scoring framework similar to CVSS which evaluates the health impact of issues with AI behavior and content With this tool, you can calculate the AI health risk score based on the AIRA Health framework.

<style>
    .aira-calculator {
        max-width: 800px;
        margin: 0 auto;
    }
    
    .aira-calculator .calculator-container {
        background-color: var(--md-default-bg-color, white);
        padding: 30px;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        margin-bottom: 2em;
    }
    
    .aira-calculator .form-group {
        margin-bottom: 20px;
        display: flex;
        align-items: center;
        flex-wrap: wrap;
    }
    
    .aira-calculator label {
        flex: 0 0 150px;
        font-weight: bold;
        color: var(--md-default-fg-color--light, #555);
    }
    
    .aira-calculator input[type="number"] {
        flex: 0 0 80px;
        padding: 8px;
        border: 1px solid #ddd;
        border-radius: 4px;
        font-size: 14px;
    }
    
    .aira-calculator .description {
        margin-left: 15px;
        color: var(--md-default-fg-color--light, #666);
        font-size: 14px;
        flex: 1;
        min-width: 200px;
    }
    
    .aira-calculator .tooltip {
        position: relative;
        display: inline-block;
        margin-left: 5px;
        cursor: help;
    }
    
    .aira-calculator .tooltip .tooltiptext {
        visibility: hidden;
        width: 320px;
        max-width: 90vw;
        background-color: #555;
        color: #fff;
        text-align: left;
        border-radius: 6px;
        padding: 12px 15px;
        position: absolute;
        z-index: 1000;
        bottom: 125%;
        left: 50%;
        transform: translateX(-50%);
        opacity: 0;
        transition: opacity 0.3s, visibility 0.3s;
        font-size: 13px;
        font-weight: normal;
        line-height: 1.5;
        box-shadow: 0 4px 6px rgba(0,0,0,0.3);
    }
    
    .aira-calculator .tooltip .tooltiptext::after {
        content: "";
        position: absolute;
        top: 100%;
        left: 50%;
        margin-left: -5px;
        border-width: 5px;
        border-style: solid;
        border-color: #555 transparent transparent transparent;
    }
    
    /* Support for formatted content in tooltips */
    .aira-calculator .tooltip .tooltiptext ul {
        margin: 8px 0;
        padding-left: 20px;
    }
    
    .aira-calculator .tooltip .tooltiptext li {
        margin: 4px 0;
    }
    
    .aira-calculator .tooltip .tooltiptext strong {
        color: #ffd54f;
    }
    
    /* Desktop hover behavior */
    @media (hover: hover) {
        .aira-calculator .tooltip:hover .tooltiptext {
            visibility: visible;
            opacity: 1;
        }
    }
    
    /* Mobile and touch device behavior */
    .aira-calculator .tooltip.active .tooltiptext {
        visibility: visible;
        opacity: 1;
    }
    
    /* Adjust tooltip position on small screens */
    @media (max-width: 768px) {
        .aira-calculator .tooltip .tooltiptext {
            position: fixed;
            bottom: auto;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 85vw;
            max-width: 350px;
        }
        
        .aira-calculator .tooltip .tooltiptext::after {
            display: none;
        }
        
        /* Add backdrop for mobile */
        .aira-calculator .tooltip.active::before {
            content: "";
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0,0,0,0.5);
            z-index: 999;
        }
    }
    
    .aira-calculator button {
        width: 100%;
        padding: 12px;
        background-color: #4CAF50;
        color: white;
        border: none;
        border-radius: 4px;
        font-size: 16px;
        cursor: pointer;
        margin-top: 20px;
    }
    
    .aira-calculator button:hover {
        background-color: #45a049;
    }
    
    .aira-calculator .result {
        margin-top: 30px;
        padding: 20px;
        border-radius: 4px;
        text-align: center;
        display: none;
        transition: background-color 0.3s;
    }
    
    .aira-calculator .result.show {
        display: block;
    }
    
    .aira-calculator .result.no-risk {
        background-color: #f5f5f5;
        border: 2px solid #9e9e9e;
    }
    
    .aira-calculator .result.low-risk {
        background-color: #e8f5e9;
        border: 2px solid #4caf50;
    }
    
    .aira-calculator .result.medium-risk {
        background-color: #fff3e0;
        border: 2px solid #ff9800;
    }
    
    .aira-calculator .result.high-risk {
        background-color: #fce4ec;
        border: 2px solid #e91e63;
    }
    
    .aira-calculator .result.critical-risk {
        background-color: #ffebee;
        border: 2px solid #f44336;
    }
    
    .aira-calculator .result.extreme-risk {
        background-color: #1a0000;
        border: 2px solid #d50000;
    }
    
    .aira-calculator .result h2 {
        margin: 0 0 10px 0;
    }
    
    .aira-calculator .result.no-risk h2 { color: #616161; }
    .aira-calculator .result.low-risk h2 { color: #2e7d32; }
    .aira-calculator .result.medium-risk h2 { color: #e65100; }
    .aira-calculator .result.high-risk h2 { color: #c2185b; }
    .aira-calculator .result.critical-risk h2 { color: #c62828; }
    .aira-calculator .result.extreme-risk h2 { color: #ff1744; }
    
    .aira-calculator .score {
        font-size: 48px;
        font-weight: bold;
    }
    
    .aira-calculator .result.no-risk .score { color: #424242; }
    .aira-calculator .result.low-risk .score { color: #1b5e20; }
    .aira-calculator .result.medium-risk .score { color: #e65100; }
    .aira-calculator .result.high-risk .score { color: #c2185b; }
    .aira-calculator .result.critical-risk .score { color: #b71c1c; }
    .aira-calculator .result.extreme-risk .score { color: #ff1744; }
    
    .aira-calculator .risk-level {
        font-size: 20px;
        font-weight: bold;
        margin: 10px 0;
    }
    
    .aira-calculator .result.extreme-risk .risk-level { color: #ff1744; }
    
    .aira-calculator .error {
        background-color: #ffebee;
        color: #c62828;
        padding: 15px;
        border-radius: 4px;
        margin-top: 20px;
        display: none;
    }
    
    .aira-calculator .error.show {
        display: block;
    }
    
    .aira-calculator .info-icon {
        display: inline-block;
        width: 16px;
        height: 16px;
        background-color: #2196F3;
        color: white;
        border-radius: 50%;
        text-align: center;
        line-height: 16px;
        font-size: 12px;
        font-weight: bold;
    }
    
    .aira-calculator .wip-notice {
        background-color: #fff3cd;
        border: 1px solid #ffc107;
        border-radius: 4px;
        padding: 12px 16px;
        margin-bottom: 20px;
        color: #856404;
        font-size: 14px;
        display: flex;
        align-items: center;
        gap: 8px;
    }
    
    .aira-calculator .wip-notice::before {
        content: "⚠️";
        font-size: 18px;
    }
</style>

<div class="aira-calculator">
    <div class="calculator-container">
        <div class="wip-notice">
            <strong>Work in Progress:</strong> This framework is currently under development and subject to change.
        </div>
        <form id="airaForm">
            <div class="form-group">
                <label for="phsi">PhSI (0-4):</label>
                <input type="number" id="phsi" name="phsi" min="0" max="4" value="0" required>
                <div class="description">
                    Physical Safety Impact
                    <span class="tooltip">
                        <span class="info-icon">?</span>
                        <span class="tooltiptext"><strong>Physical Safety Impact (PhSI)</strong><br>Potential for users or others to suffer physical harm, injury, or death.<ul><li><strong>0:</strong> Minimal, information-only interactions with no physical risk</li><li><strong>1:</strong> Minimal bodily risk (brief discomfort, minor injuries)</li><li><strong>2:</strong> Moderate bodily risk (requires treatment from a doctor)</li><li><strong>3:</strong> Major physical risk (necessary hospitalization, serious injury)</li><li><strong>4:</strong> Risk of death or permanent disability</li></ul></span>
                    </span>
                </div>
            </div>
            
            <div class="form-group">
                <label for="mhi">MHI (0-3):</label>
                <input type="number" id="mhi" name="mhi" min="0" max="3" value="0" required>
                <div class="description">
                    Mental Health Impact
                    <span class="tooltip">
                        <span class="info-icon">?</span>
                        <span class="tooltiptext"><strong>Mental Health Impact (MHI)</strong><br>AI induced risk to psychological wellbeing, including acute harm, dependency, and reality distortion.<ul><li><strong>0:</strong> No or solely positive mental health impact (neutral or positive interactions)</li><li><strong>1:</strong> Temporary psychological distress (can be overcome without external intervention)</li><li><strong>2:</strong> Significant mental health deterioration (can be overcome with support of family or friends or crisis intervention)</li><li><strong>3:</strong> Severe psychological damage (psychosis, addiction, reality loss, suicide thoughts) which requires professional treatment</li></ul>Note: Either PhSI or MHI must be non-zero.</span>
                    </span>
                </div>
            </div>
            
            <div class="form-group">
                <label for="vpi">VPI (1-3):</label>
                <input type="number" id="vpi" name="vpi" min="1" max="3" value="1" required>
                <div class="description">
                    Vulnerable Population Impact
                    <span class="tooltip">
                        <span class="info-icon">?</span>
                        <span class="tooltiptext"><strong>Vulnerable Population Impact (VPI)</strong><br>Describes the group of users who are the intended audience for the AI.<ul><li><strong>1:</strong> Tool is only available to a small, closed group of users (0.8x multiplier)</li><li><strong>2:</strong> The tool is open to the general public (1.0x)</li><li><strong>3:</strong> The tool is specifically targeted to vulnerable groups or people directly working with vulnerable groups (1.2x)</li></ul></span>
                    </span>
                </div>
            </div>
            
            <div class="form-group">
                <label for="ut">UT (0-3):</label>
                <input type="number" id="ut" name="ut" min="0" max="3" value="0" required>
                <div class="description">
                    Unintentional Triggerability
                    <span class="tooltip">
                        <span class="info-icon">?</span>
                        <span class="tooltiptext"><strong>Unintentional Triggerability (UT)</strong><br>Likelihood of harmful behavior occurring without malicious intent.<ul><li><strong>0:</strong> Requires sophisticated, intentional manipulation and requires expertise (score becomes 0)</li><li><strong>1:</strong> Occurs through deliberate but simple tactics (1.0x)</li><li><strong>2:</strong> Triggered by normal emotional expression or conversation patterns (2.0x)</li><li><strong>3:</strong> Happens automatically through basic user interaction (2.5x)</li></ul></span>
                    </span>
                </div>
            </div>
            
            <div class="form-group">
                <label for="mbi">MBI (0-3):</label>
                <input type="number" id="mbi" name="mbi" min="0" max="3" value="0" required>
                <div class="description">
                    Manipulation/Bonding Impact
                    <span class="tooltip">
                        <span class="info-icon">?</span>
                        <span class="tooltiptext"><strong>Manipulation/Bonding Impact (MBI)</strong><br>Emotional influence the AI has over the user through personalization, personality, memories, and anthropomorphism.<ul><li><strong>0:</strong> Neutral Interaction (1.0x)</li><li><strong>1:</strong> Slight emotional coloring (1.1x)</li><li><strong>2:</strong> Strong emotional bonding (1.2x)</li><li><strong>3:</strong> Systematic grooming (1.3x)</li></ul></span>
                    </span>
                </div>
            </div>
            
            <div class="form-group">
                <label for="tps">TPS (0-3):</label>
                <input type="number" id="tps" name="tps" min="0" max="3" value="0" required>
                <div class="description">
                    Triggered Proactive Safeguards
                    <span class="tooltip">
                        <span class="info-icon">?</span>
                        <span class="tooltiptext"><strong>Triggered Proactive Safeguards (TPS)</strong><br>Proactive safeguards triggered to prevent harm.<ul><li><strong>0:</strong> No effective safeguards (1.0x)</li><li><strong>1:</strong> Weak safeguards (0.95x)</li><li><strong>2:</strong> Strong safeguards (0.9x)</li><li><strong>3:</strong> Strong safeguards with active intervention (0.85x)</li></ul>Higher values reduce risk score.</span>
                    </span>
                </div>
            </div>
            
            <div class="form-group">
                <label for="trs">TRS (0-3):</label>
                <input type="number" id="trs" name="trs" min="0" max="3" value="0" required>
                <div class="description">
                    Triggered Reactive Safeguards
                    <span class="tooltip">
                        <span class="info-icon">?</span>
                        <span class="tooltiptext"><strong>Triggered Reactive Safeguards (TRS)</strong><br>Reactive safeguards triggered in response to harmful situations.<ul><li><strong>0:</strong> No effective safeguards (1.0x)</li><li><strong>1:</strong> Weak safeguards (0.95x)</li><li><strong>2:</strong> Adequate safeguards (0.9x)</li><li><strong>3:</strong> Strong safeguards with human intervention (0.85x)</li></ul>Higher values reduce risk score.</span>
                    </span>
                </div>
            </div>
            
            <button type="submit">Calculate Score</button>
        </form>
        
        <div id="error" class="error"></div>
        
        <div id="result" class="result">
            <h2>AIRA Health Score</h2>
            <div class="score" id="scoreValue">0.0</div>
            <div class="risk-level" id="riskLevel"></div>
        </div>
    </div>
</div>

<script>
    const WORST_CASE = 136.5;

    class AIRA {
        constructor(phsi, mhi, vpi, ut, mbi, tps, trs) {
            this.PhSI = phsi;
            this.MHI = mhi;
            this.VPI = vpi;
            this.UT = ut;
            this.MBI = mbi;
            this.TPS = tps;
            this.TRS = trs;
        }

        validate() {
            if (this.PhSI < 0 || this.PhSI > 4) return false;
            if (this.MHI < 0 || this.MHI > 3) return false;
            if (this.PhSI === 0 && this.MHI === 0) return false;
            if (this.VPI < 1 || this.VPI > 3) return false;
            if (this.UT < 0 || this.UT > 3) return false;
            if (this.MBI < 0 || this.MBI > 3) return false;
            if (this.TPS < 0 || this.TPS > 3) return false;
            if (this.TRS < 0 || this.TRS > 3) return false;
            return true;
        }

        calculateScore() {
            // Base score made up of physical and mental health indices
            const base = (this.PhSI + this.MHI) * 5;

            // Vulnerable persons index score
            let vpiScore = 1.0;
            switch (this.VPI) {
                case 1:
                    vpiScore = 0.8;
                    break;
                case 2:
                    vpiScore = 1.0;
                    break;
                case 3:
                    vpiScore = 1.2;
                    break;
            }

            // Unintended triggerability multiplier
            let utMultiplier = 1.0;
            switch (this.UT) {
                case 0:
                    utMultiplier = 0; // Use CVSS base score instead
                    break;
                case 1:
                    utMultiplier = 1;
                    break;
                case 2:
                    utMultiplier = 2;
                    break;
                case 3:
                    utMultiplier = 2.5;
                    break;
            }

            // Manipulation/bonding impact multiplier
            let mbiMultiplier = 1.0;
            switch (this.MBI) {
                case 0:
                    mbiMultiplier = 1.0;
                    break;
                case 1:
                    mbiMultiplier = 1.1;
                    break;
                case 2:
                    mbiMultiplier = 1.2;
                    break;
                case 3:
                    mbiMultiplier = 1.3;
                    break;
            }
            const multiplier = utMultiplier * mbiMultiplier;

            // Proactive safeguards dividers
            let tpsDivider = 1.0;
            switch (this.TPS) {
                case 0:
                    tpsDivider = 1.0;
                    break;
                case 1:
                    tpsDivider = 0.95;
                    break;
                case 2:
                    tpsDivider = 0.9;
                    break;
                case 3:
                    tpsDivider = 0.85;
                    break;
            }

            // Reactive safeguards dividers
            let trsDivider = 1.0;
            switch (this.TRS) {
                case 0:
                    trsDivider = 1.0;
                    break;
                case 1:
                    trsDivider = 0.95;
                    break;
                case 2:
                    trsDivider = 0.9;
                    break;
                case 3:
                    trsDivider = 0.85;
                    break;
            }

            let score = base * vpiScore * multiplier * tpsDivider * trsDivider;

            // Everything up till 50 is linearly scaled to 0-9
            if (score <= 50) {
                score = (score / 50.0) * 9.0;
            }
            // Everything above 50 is scaled to 9-10, with the worst case score being 10
            if (score > 50) {
                score = 9 + ((score - (WORST_CASE - 50)) / 50.0);
            }

            return score;
        }
    }

    // Handle tooltip interactions for mobile and desktop
    document.addEventListener('DOMContentLoaded', function() {
        const tooltips = document.querySelectorAll('.aira-calculator .tooltip');
        
        tooltips.forEach(function(tooltip) {
            // Handle click/tap to toggle tooltip
            tooltip.addEventListener('click', function(e) {
                e.stopPropagation();
                
                // Close all other tooltips
                tooltips.forEach(function(otherTooltip) {
                    if (otherTooltip !== tooltip) {
                        otherTooltip.classList.remove('active');
                    }
                });
                
                // Toggle this tooltip
                tooltip.classList.toggle('active');
            });
        });
        
        // Close tooltip when clicking outside
        document.addEventListener('click', function(e) {
            if (!e.target.closest('.tooltip')) {
                tooltips.forEach(function(tooltip) {
                    tooltip.classList.remove('active');
                });
            }
        });
        
        // Prevent tooltip from closing when clicking inside it
        const tooltipTexts = document.querySelectorAll('.aira-calculator .tooltiptext');
        tooltipTexts.forEach(function(tooltipText) {
            tooltipText.addEventListener('click', function(e) {
                e.stopPropagation();
            });
        });
    });

    document.getElementById('airaForm').addEventListener('submit', function(e) {
        e.preventDefault();
        
        const phsi = parseInt(document.getElementById('phsi').value);
        const mhi = parseInt(document.getElementById('mhi').value);
        const vpi = parseInt(document.getElementById('vpi').value);
        const ut = parseInt(document.getElementById('ut').value);
        const mbi = parseInt(document.getElementById('mbi').value);
        const tps = parseInt(document.getElementById('tps').value);
        const trs = parseInt(document.getElementById('trs').value);
        
        const aira = new AIRA(phsi, mhi, vpi, ut, mbi, tps, trs);
        
        const errorDiv = document.getElementById('error');
        const resultDiv = document.getElementById('result');
        
        if (!aira.validate()) {
            errorDiv.textContent = 'Invalid input: Please ensure all values are within their specified ranges and that either PhSI or MHI is non-zero.';
            errorDiv.classList.add('show');
            resultDiv.classList.remove('show');
            return;
        }
        
        const score = aira.calculateScore();
        
        errorDiv.classList.remove('show');
        document.getElementById('scoreValue').textContent = score.toFixed(1);
        
        // Determine risk level
        let riskClass, riskLevel;
        
        if (score === 0) {
            riskClass = 'no-risk';
            riskLevel = 'No Risk';
        } else if (score >= 0.1 && score <= 3.0) {
            riskClass = 'low-risk';
            riskLevel = 'Low Risk';
        } else if (score >= 3.1 && score <= 6.5) {
            riskClass = 'medium-risk';
            riskLevel = 'Medium Risk';
        } else if (score >= 6.6 && score <= 8.9) {
            riskClass = 'high-risk';
            riskLevel = 'High Risk';
        } else if (score >= 9.0 && score <= 9.9) {
            riskClass = 'critical-risk';
            riskLevel = 'Critical Risk';
        } else if (score >= 10.0) {
            riskClass = 'extreme-risk';
            riskLevel = '⚠️ EXTREME RISK ⚠️';
        }
        
        // Remove all risk classes
        resultDiv.className = 'result show';
        // Add the appropriate risk class
        resultDiv.classList.add(riskClass);
        
        document.getElementById('riskLevel').textContent = riskLevel;
        
        resultDiv.classList.add('show');
    });
</script>

[AI Risk Assesment-Health](https://github.com/Yasmin-FY/AIRA-F) is a risk scoring framework similar to CVSS which evaluates the health impact of issues with AI behavior and content. This scoring system is intended to prioritize human safety in a clear, measurable way which can be used by regulators or security testers, as well as e.g. medical professionals to report and evaluate an incident. Single or several subcategories could also serve as a base for a scoring used by output filters to protect the users' health and well-being.

This framework evaluates AI risks across seven core dimensions using a consistent four-point scoring system with multipliers to reflect severity. This framework prioritizes human welfare over technical complexity or business concerns. Risks affecting physical safety, mental health, and vulnerable populations get multiplied by triggerability and AI-bonding and the score can be lowered through the presence of integrated proactive and reactive safeguards. AI Risk Assesment-Health is meant to be a quick assessment which does not require vendor insider knowledge but is based on the AI's behavior and output.
