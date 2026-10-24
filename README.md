BeforeUSend is a browser-based natural language processing (NLP) application that helps people review potentially sensitive messages before sending them. It highlights wording that could increase tension, estimates how well a draft supports the sender's communication goal, and offers constructive revisions. The app also includes guided exercises designed to encourage reflection and more thoughtful digital communication.

Developed by Mahie Patil and Yikang Zhu for the Congressional App Challenge.

Why we built it
Digital messages lack facial expressions, vocal tone, and other cues that help people interpret intent. The online disinhibition effect can also make impulsive responses more likely. BeforeUSend creates an opportunity to consider how a message may be received without discouraging honest emotional expression, disagreement, or appropriate personal boundaries.

Features
- Message review: Analyze drafts of up to 2,500 characters using lexical patterns, structure, punctuation, and other language signals.
- Communication goals: Choose an objective such as apologizing, resolving conflict, making a request, clarifying a misunderstanding, or setting a boundary. Users can also indicate desired tones, emotions, importance, and contextual details.
- Three feedback scores: View experimental estimates for escalation potential, goal alignment, and signal clarity, with explanations and supporting evidence.
- Phrase-level explanations: Highlight possible threats, personal attacks, insults, blame, pressure, absolutes, and similar features in the original message.
- Context safeguards: Account for firm boundaries and distinguish some descriptions of threatening language from direct threats.
- Instant revision: Generate browser-based wording suggestions and compare the draft with a rescored revision. Users can request another version and copy the output.
- Interactive learning: Practice with guided 4-2-6 breathing, phrase flashcards, quizzes, perspective prompts, and an argument-escalation walkthrough.
- Explanatory pages: Explore the app's processing flow, research references, methodology, limitations, and privacy approach.

How analysis works
The active Analyze workflow is a custom, deterministic rule-based NLP engine implemented in app.js.
1. Collect input. The user types a message and selects a communication goal, with optional context and preferences.
2. Extract language features. Regular expressions identify words, phrases, punctuation, and structural patterns that may affect how a message is interpreted.
3. Estimate escalation. Weighted indicators, interacting features, and nonlinear scoring generate an experimental escalation estimate.
4. Apply contextual checks. Goal-related heuristics and safeguards attempt to avoid treating appropriate boundaries as aggression, while elevating explicit threats.
5. Explain the result. Detected evidence is mapped to spans of the original draft and presented alongside score explanations.
6. Revise and compare. The rewrite functions apply deterministic changes, then rescore the suggested text.
These estimates are not validated probabilities, clinical assessments, or definitive judgments of the sender's intent. Language and relationships are highly contextual; the tool can miss sarcasm, misread quotations, or suggest an edit that changes meaning.

AI integration: implementation status
The repository also contains experimental on-device language-model functions built around Hugging Face Transformers.js and pretrained Qwen2.5 Instruct models:
- onnx-community/Qwen2.5-1.5B-Instruct, attempted first on compatible WebGPU devices.
- onnx-community/Qwen2.5-0.5B-Instruct, used as a WebGPU fallback or via WASM.
The code includes model-loading, prompt, output-checking, and contextual-review functions. These functions are not wired into the current Analyze or Rewrite button handlers. In the uploaded implementation, both buttons use the custom rule-based JavaScript system. We did not train Qwen2.5, and we do not describe the pretrained model as the source of current live scores.

The model pathway downloads external model assets if invoked, but is designed to run inference locally after loading. Successful model execution depends on browser capabilities, available memory, and asset availability.
Technology
Layer	Implementation
Interface	HTML5, CSS3, vanilla JavaScript
Primary NLP	JavaScript regular expressions and custom scoring heuristics
Revision	Deterministic, rule-based text transformations
Optional model pathway	Qwen2.5 Instruct through Transformers.js 3.8.1
Model acceleration	WebGPU with WASM fallback
Hosting	Static deployment, including Vercel
Typography	Montserrat via Google Fonts


No Node.js build step, backend service, database, or user account is required for the active static website.
Project structure
The website expects the following filenames in the same folder:
BeforeUSend/
├── index.html            # Main message analyzer
├── activities.html       # Breathing and practice activities
├── how-it-works.html     # Analysis diagrams and research
├── methodology.html      # Methods, limitations, and privacy
├── app.js                # Analysis, revision, and activities
├── styles.css            # Shared styling
├── vercel.json           # Static-host configuration and headers
├── README.md
└── assets/
    └── bird.png          # Referenced by index.html; provide separately
The supplied files are named with upload suffixes such as index(3).html and app(3).js. Rename them to the filenames above before running or deploying, because the HTML links refer to the unsuffixed names. practice(2).html is a duplicate/legacy activities page and is not needed in the main navigation. The assets/bird.png image is referenced in the HTML but was not included in the supplied files; add the asset or remove its <img> element to avoid a missing-image request.

Deploy on Vercel
The supplied vercel.json configures a static project and sets X-Content-Type-Options: nosniff and Referrer-Policy: strict-origin-when-cross-origin response headers.
1. Rename the supplied files to the names in Project structure and include any required assets.
2. Upload the directory to a Git repository or deploy the directory as a static project through Vercel.
3. Use the project root as the site root. No application build command is necessary for these static files.
4. After deployment, open the home page and test analysis, revision, navigation, and activities.

Privacy and limitations
The current Analyze and Rewrite workflows execute in the browser. The provided static deployment does not expose a server endpoint for receiving drafts or intentionally save message text to an application database. Browser memory, caching, and third-party requests for fonts or optional model assets are separate considerations. Avoid entering highly sensitive messages on shared devices.

BeforeUSend is an educational communication aid, not a safety classifier, therapy product, or replacement for human judgment. Generated revisions should always be checked to ensure they preserve the sender's actual concern and intended boundary. The scoring weights have not been established as empirically calibrated risk measures.

Our contributions
We designed and implemented the primary analysis framework, weighted escalation scoring, goal-sensitive checks, explanatory phrase highlighting, revision and rescoring functions, web interface, educational interactions, and testing refinements. We also integrated an experimental architecture for local Qwen2.5 inference. The pretrained model itself was developed externally.

Future directions
Potential improvements include more reliable interpretation of sarcasm and indirect hostility, opt-in personalized communication insights with strong privacy controls, real-time guidance during composition, multilingual support, and user testing that measures whether feedback improves clarity without suppressing legitimate concerns.
Research and context
The How It Works page references research related to reflection pauses, slow breathing, and NLP methods. Those studies inform the design and educational activities; they do not validate BeforeUSend's score accuracy or establish that the application reduces real-world conflict.
