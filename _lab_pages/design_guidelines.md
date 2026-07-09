---
layout: post
title:  AI Technology Design Guideline Explorer
date:   2026-07-09
categories: aloe
permalink: /design_guidelines/
author: Glen Smith
comments: false
---

<p>
An interactive search tool to explore design guidelines for AI technologies that support Adult Learners.
</p>

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap');

        html {
            scroll-behavior: smooth;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8fafc;
        }

        .page-content .wrapper {
            max-width: initial;
        }

        .control-panel {
            backdrop-filter: blur(16px);
            background: rgba(255, 255, 255, 0.92);
            -webkit-backdrop-filter: blur(16px);
            border-bottom: 1px solid #e2e8f0;
            position: sticky;
            top: 0;
            z-index: 50;
            transition: box-shadow 0.3s ease-in-out;
            box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08);
        }

        .custom-checkbox:checked {
            background-image: url("data:image/svg+xml,%3csvg viewBox='0 0 16 16' fill='white' xmlns='http://www.w3.org/2000/svg'%3e%3cpath d='M12.207 4.793a1 1 0 010 1.414l-5 5a1 1 0 01-1.414 0l-2-2a1 1 0 011.414-1.414L6.5 9.086l4.293-4.293a1 1 0 011.414 0z'/%3e%3c/svg%3e");
        }

        .filter-collapse {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.35s ease-in-out, opacity 0.3s ease-in-out;
            opacity: 0;
        }

        .filter-collapse.expanded {
            max-height: 500px;
            opacity: 1;
            overflow: visible;
        }

        .collapse-toggle-icon {
            transition: transform 0.3s ease-in-out;
        }

        .collapse-toggle-icon.rotated {
            transform: rotate(180deg);
        }

        .tag-clickable {
            cursor: pointer;
            transition: transform 0.15s ease, box-shadow 0.15s ease;
        }

        .tag-clickable:hover {
            transform: scale(1.08);
            box-shadow: 0 2px 6px rgba(0, 0, 0, 0.12);
        }

        .tag-clickable.active-filter {
            ring: 2px;
            box-shadow: 0 0 0 2px rgba(99, 102, 241, 0.6);
        }

        #sb-overlay {
            position: fixed;
            inset: 0;
            background: rgba(15, 23, 42, .45);
            z-index: 99;
            opacity: 0;
            pointer-events: none;
            transition: opacity .35s ease
        }

        #sb-overlay.sb-on {
            opacity: 1;
            pointer-events: all
        }

        #sb-panel {
            position: fixed;
            top: 0;
            right: 0;
            bottom: 0;
            width: 460px;
            max-width: 100%;
            background: #fff;
            z-index: 100;
            display: flex;
            flex-direction: column;
            transform: translateX(100%);
            transition: transform .35s cubic-bezier(.4, 0, .2, 1);
            box-shadow: -4px 0 40px rgba(0, 0, 0, .15);
            overflow: hidden
        }

        #sb-panel.sb-open {
            transform: translateX(0)
        }

        #sb-header {
            padding: 20px;
            border-bottom: 1px solid #e2e8f0;
            flex-shrink: 0
        }

        #sb-body {
            flex: 1;
            overflow-y: auto;
            padding: 20px
        }
    </style>
</head>

<body class="antialiased text-gray-800">

    <div id="root"></div>

    <script>
        var quotesDB = {
            "i want to know what data is being collected about me and have control over how it will be used so i can balance privacy with benefits": [{ text: "Learners need transparency and control over where their data goes, how its stored and ability to manage or delete it.", labels: ["Learner", "Researcher"] }, { text: "I want to know what data is being collected.", labels: ["Learner"] }, { text: "Learning technologies must prioritize privacy, avoid unnecessary data sharing and protect sensitive information,even from sharing with private LLMs.", labels: ["Developer", "Researcher"] }, { text: "Trust and clear boundaries about data privacy are essential.", labels: ["Learner", "Researcher"] }, { text: "data collection must follow standards, and balance access with protection.", labels: ["Developer", "Researcher"] }],
            "my tools should adopt good design principles that make learning accessible to everyone": [{ text: "I need technologies that help with my learning disability.", labels: ["Learner"] }, { text: "My technologies are accessible to everyone.", labels: ["Developer"] }, { text: "I want to build accessible technologies.", labels: ["Developer"] }, { text: "AI tools should adopt good interface design principles.", labels: ["Developer", "Researcher"] }],
            "i should be able to access ai help on a phone with or without internet and it should be affordable": [{ text: "Some students don't have internet.", labels: ["Learner", "Instructor"] }, { text: "I learn using mobile devices.", labels: ["Learner"] }, { text: "My tools should be scalable and cost-effective.", labels: ["Developer"] }],
            "i an adult have limited time and resources and learning needs to to fit into my life": [{ text: "Adult learners mean business and activities need to be aligned to be worth their time (even social stuff).", labels: ["Researcher", "Instructor"] }, { text: "Adults have limited time.", labels: ["Learner", "Researcher"] }, { text: "Adults have limited resources.", labels: ["Learner", "Researcher"] }],
            "i think ai's ability to provide extra help anytime anywhere makes it super human": [{ text: "The tool can provide \"superhuman\" help that the teacher can't.", labels: ["Learner"] }, { text: "I want to access the technologies anytime.", labels: ["Learner"] }, { text: "I want to access the technologies anywhere.", labels: ["Learner"] }, { text: "My parents can't help me with my homework, maybe AI can.", labels: ["Learner"] }],
            "i think learning technologies should be informed by learning science and learning theories": [{ text: "AI tool should be grounded in established motivation theories, emphasizing relatedness and competenmce.", labels: ["Researcher"] }, { text: "Theory suggests people learn better from multiple linked modalities.", labels: ["Researcher"] }, { text: "We want to help learners understand concepts by using mental model theory.", labels: ["Developer", "Researcher"] }, { text: "I think effective learning design should align with cognitive processing principles, reducing overload while promoting deeper understanding.", labels: ["Researcher"] }, { text: "I need learning design to be guided by established cognitive and multimedia learning theories that strengthen comprehension and knowledge constriction.", labels: ["Researcher"] }, { text: "I want to design tools that are grounded in theory.", labels: ["Developer", "Researcher"] }, { text: "I want to consider design when building learning tools.", labels: ["Developer", "Researcher"] }],
            "the ai tool has a big learning curve and i need an introduction tutorial on what it can do and how to use it": [{ text: "I want to understand the capabilities of my AI systems.", labels: ["Learner"] }, { text: "The learning curve is a big barrier to using the AI tool.", labels: ["Learner"] }, { text: "An introduction to the tool would help me use it better.", labels: ["Learner"] }, { text: "I want a tutorial for the tool.", labels: ["Learner", "Instructor"] }],
            "i need an ai tool that works without errors and is easy for me to understand quickly or i won't use it because my time is better spent elsewhere": [{ text: "I feel frustrated by the tool's interface.", labels: ["Learner"] }, { text: "I want to design tools that reduce frustration.", labels: ["Developer"] }, { text: "All the clicking between screens is a barrier to using the tool.", labels: ["Learner", "Instructor"] }, { text: "I want the tool to work, and work without errors or friction.", labels: ["Learner"] }, { text: "I (the teacher) have to spend time fixing AI issues.", labels: ["Instructor"] }, { text: "I did not enourage use of the AI tool because I was uncomfortable with it or didn't have enough time.", labels: ["Instructor"] }],
            "i want to design ai tools that are intuitive easy to use and promote natural interactions to avoid confusion over the interface and expectations of the tool": [{ text: "I couldn't figure out how to use the tool and what it expected of me.", labels: ["Learner"] }, { text: "it is unclear how the tutor works.", labels: ["Learner"] }, { text: "I make mistakes because of the design of the UI.", labels: ["Learner", "Instructor"] }, { text: "The tool's UI is not intuitive to me.", labels: ["Learner"] }, { text: "I want to design intuitive technologies.", labels: ["Developer"] }, { text: "I want to design tools that align with natural behaviors and interactions.", labels: ["Developer", "Researcher"] }, { text: "I want to design tools that are easy to use.", labels: ["Developer"] }, { text: "I'm afraid of accidentally deleting my work.", labels: ["Learner", "Instructor"] }],
            "i want to understand how the ai works and i want it to better understand me mutual theory of mind": [{ text: "I want to be able to understand the AI.", labels: ["Learner"] }, { text: "I don't think the AI understands me.", labels: ["Learner"] }, { text: "I wish the tool was easy to understand.", labels: ["Learner"] }],
            "i want explanations for why my answers are wrong and or how to do it": [{ text: "I want to design tools that provide good explanations to students.", labels: ["Developer"] }, { text: "I want detailed explanations.", labels: ["Learner"] }, { text: "I want the tutor to explain concepts.", labels: ["Learner"] }],
            "i want to know why the ai thinks my answer is wrong": [{ text: "I want to know why I'm wrong.", labels: ["Learner"] }, { text: "I want to know why my response was wrong.", labels: ["Learner"] }, { text: "I don't understand why the AI wont accept my answer.", labels: ["Learner"] }, { text: "I want to know why the AI says things.", labels: ["Learner"] }, { text: "The AI uses multiple (specific) evaluation mechanisms to score student responses.", labels: ["Developer", "Researcher"] }],
            "i want to build tools that utilize design principles to direct attention and recall information where it is needed": [{ text: "The tool directs my attention to what I need to work on.", labels: ["Learner"] }, { text: "I want the visual features to make important information clear.", labels: ["Developer"] }, { text: "I need AI tools to help me retain and/or recall information.", labels: ["Learner"] }],
            "i want tools that are simple and do not unnecessarily increase cognitive load because i believe it will motivate students to use the tool more": [{ text: "I want to reduce cognitive load for adult learners.", labels: ["Researcher"] }, { text: "I want to reduce cognitive load by designing tools that are easy to use.", labels: ["Developer"] }, { text: "We should design tools that reduce cognitive load.", labels: ["Developer", "Researcher"] }, { text: "I want to simplify information.", labels: ["Developer"] }, { text: "I think superfluous things are distracting and hurt engagement, motivation, and learning.", labels: ["Researcher"] }, { text: "I want to design simple UIs.", labels: ["Developer"] }, { text: "I want to design tools that are beautiful.", labels: ["Developer"] }, { text: "I believe that decreasing cognitive load will increase tool adoption.", labels: ["Researcher"] }, { text: "I (AI tools) help students connect the dots between concepts.", labels: ["Developer", "Researcher"] }],
            "i want to options choices for how i engage with content as well as access to supplementary content": [{ text: "I want the tool to link to supplementary learning content.", labels: ["Learner", "Instructor"] }, { text: "I want AI to provide me with more ways to engage with the class content (e.g., summaries, translating text, giving me hands-on practice).", labels: ["Learner"] }, { text: "I want support for multiple ways to solve problems.", labels: ["Learner"] }, { text: "I want to be able to customize and have control over how the tool supports me.", labels: ["Learner"] }, { text: "The tool should be optional, using it shouldn't make extra work.", labels: ["Learner"] }],
            "i want to have agency in my learning": [{ text: "I want to give learners control over their learning process/outcomes.", labels: ["Researcher", "Developer"] }, { text: "I want students learning to be self-directed.", labels: ["Instructor"] }, { text: "I want to see cognitive designs that reduce fragmentation by fostering meta cognitive skills, communication and self-directed learning.", labels: ["Researcher"] }, { text: "encouraging autonomy and self-direction through flexibility and responsibility.", labels: ["Researcher"] }, { text: "I want to have agency in my learning.", labels: ["Learner"] }, { text: "I want to have agency in my learning.", labels: ["Learner"] }, { text: "Our tools should give learners control over their learning journey.", labels: ["Developer"] }],
            "i want to build tools that coach meta cognitive strategies such as summarization pre writing self explanation hypothesis testing": [{ text: "I want to build tools that promote metacognitive skills.", labels: ["Developer"] }, { text: "I want learners to self-explain to improve their learning.", labels: ["Researcher"] }, { text: "Students learn better through self-explanation.", labels: ["Researcher"] }, { text: "I want to support learners on developing reading strategies.", labels: ["Researcher", "Developer"] }, { text: "I want to help learners develop reading strategies.", labels: ["Researcher", "Developer"] }, { text: "We can help student improve their writing strategies and competencies.", labels: ["Researcher", "Developer"] }, { text: "I need to learn modeling & scientific thinking strategies.", labels: ["Learner"] }, { text: "We can help students improve reading comprehension by implementing effective reading strategies.", labels: ["Researcher"] }, { text: "I believe that having learners construct summaries when reading leads to improved learning.", labels: ["Researcher"] }],
            "i want my ai tools to cultivate reflective and critical thinking skills": [{ text: "I (AI tools) should help students develop critical and common-sense thinking by engaging in inference.", labels: ["Developer", "Researcher"] }, { text: "I want students to develop transferable critical thinking skills.", labels: ["Instructor", "Researcher"] }, { text: "I want to build tools that promote higher-order critical thinking.", labels: ["Developer"] }, { text: "I want to support learners in reflecting on their knowledge.", labels: ["Researcher", "Developer"] }],
            "i think engagement improves learning so we use several techniques to drive ai tool engagement motivational messages feedback social connections personalization competition and intrinsic extrinsic rewards": [{ text: "I believe engagement helps students learning.", labels: ["Researcher"] }, { text: "I want to design tools that are fun and enjoyable to users and that invoke positive affect.", labels: ["Developer"] }, { text: "The tool should use positive, motivational messages.", labels: ["Developer", "Researcher"] }, { text: "My motivation is driven by rewards and encouragement.", labels: ["Learner"] }, { text: "I want to leverage social comparison with peers to enhance engagement.", labels: ["Researcher", "Developer"] }],
            "i want to view my self as successful so progress motivates me and being told i'm wrong feels terrible": [{ text: "I want to build AI tools that increase learner confidence, motivation, and initiative by affirming their understanding of course material.", labels: ["Developer", "Researcher"] }, { text: "I want to feel self-efficacious.", labels: ["Learner"] }, { text: "I feel terrible when I make mistakes or am wrong.", labels: ["Learner"] }, { text: "I feel frustrated when I can't make progress.", labels: ["Learner"] }, { text: "I need design that strengthen competence by making progress visible and rewarding.", labels: ["Researcher", "Developer"] }, { text: "I think it would help students if they could see their progress.", labels: ["Instructor"] }, { text: "I want emotional support.", labels: ["Learner"] }],
            "i want tools that scaffold problem solving step by step": [{ text: "I need step-by-step guidance.", labels: ["Learner"] }, { text: "I want to design tools that scaffold student learning.", labels: ["Developer"] }, { text: "I progress gradually.", labels: ["Learner"] }, { text: "I like learning in bite-sized chunks.", labels: ["Learner"] }, { text: "I want step-by-step feedback.", labels: ["Learner"] }],
            "i want tools that engage students in interactv inquiry questioning through a conversational discussion based chatbot": [{ text: "I want students to engage in inquiry-based learning.", labels: ["Instructor", "Researcher"] }, { text: "I want to support group-based problem solving through inquiry, debate, and discussion.", labels: ["Researcher", "Developer"] }, { text: "I want to see cognitive presence supported beyond passive reading and recall, encouraging deeper knowledge building and inquiry-based engagement.", labels: ["Researcher"] }, { text: "Inquiry-based learning needs to be scaffolded.", labels: ["Researcher"] }, { text: "I think a chatbot will make asynchronous/ independent online learning more personable.", labels: ["Researcher", "Developer"] }, { text: "I want to design a chatbot to support learning through elaboration and explanation.", labels: ["Developer"] }, { text: "I want the AI tool to engage me in conversation.", labels: ["Learner"] }],
            "i want to collaborate with my peers to improve my learning": [{ text: "I want to have study groups.", labels: ["Learner"] }, { text: "I want students to learn collaboratively.", labels: ["Instructor"] }, { text: "I see the value in connecting to other students for learning.", labels: ["Learner"] }, { text: "I think interacting with people makes learning easier and better.", labels: ["Learner"] }, { text: "The tool should model how to effectively collaborate.", labels: ["Developer", "Researcher"] }],
            "i design generative tasks that require learners to restructure knowledge and create their own understanding": [{ text: "My technology uses summarization tasks as a way of engaging learners with the material.", labels: ["Developer"] }, { text: "I (AI tools) should support generative learning.", labels: ["Developer", "Researcher"] }, { text: "I want to help learners revise their work.", labels: ["Instructor", "Developer"] }, { text: "We should design our tools around constructivist learning theory.", labels: ["Researcher"] }, { text: "I want to give students formative feedback.", labels: ["Instructor"] }],
            "i structure learning experiences to promote active and interactive engagement over passive consumption": [{ text: "I need learning designs that strengthen cognitive presence through interaction and constructive knowledge-building tasks.", labels: ["Researcher"] }, { text: "AI tools should manage learning through structured engagement such as interactivity.", labels: ["Researcher", "Developer"] }, { text: "I use ICAP to design learning tools.", labels: ["Researcher", "Developer"] }, { text: "I build tools to be active, rather than passive.", labels: ["Developer"] }, { text: "I (AI tools) should help learners actively (non-passively) build and retain knowledge.", labels: ["Developer", "Researcher"] }, { text: "Conceptual models make interactive (ICAP) level learning activities easier.", labels: ["Researcher"] }],
            "i want to build tools that personalize to learner knowledge adapting the learning sequence and level of difficulty": [{ text: "I think AI personalization will drive improved engagement and cognitive presense.", labels: ["Researcher"] }, { text: "I want AI tools to provide an appropriate level of difficulty.", labels: ["Learner", "Developer"] }, { text: "I want the tool to be aware of what I know and what I don't.", labels: ["Learner"] }, { text: "I want personalized learning sequences that match my skills and pace.", labels: ["Learner"] }, { text: "I want to see personalization that improves learning outcomes, persistence and satisfaction.", labels: ["Researcher"] }, { text: "I think personalization should be data-informed, adaptive and human-centric to support diverse learners.", labels: ["Researcher"] }, { text: "I want the system to personalize based on learners' foundational skills and cognitive capacities.", labels: ["Researcher", "Developer"] }],
            "adults have varied knowledge expertise and needs and personalization is more important for adults learners than k12 learners": [{ text: "Adult learners require different personalization methods.", labels: ["Researcher"] }, { text: "Everyone has different needs, adults especially so.", labels: ["Researcher"] }],
            "i want my learning to support my career": [{ text: "I want to make connections to build my professional network.", labels: ["Learner"] }, { text: "I like being matched socially based on professional interests, hobbies, or proximity.", labels: ["Learner"] }, { text: "I want AI to support my job growth.", labels: ["Learner"] }, { text: "I want to learn more skills for my career.", labels: ["Learner"] }, { text: "Tool benefits should extend to my real professional life.", labels: ["Learner"] }],
            "i find learning more engaging when it is aligned to my goals": [{ text: "It is easier to stay motivated/engaged when the tool is aligned with my learning goals.", labels: ["Learner"] }, { text: "I want to see personalization that considers learners' beliefs, motivations and goals.", labels: ["Researcher"] }, { text: "I think data should be used to infer user goals and evaluate tool alignment with user goals.", labels: ["Researcher", "Developer"] }],
            "i want to practice meaningful problems relevant to my context": [{ text: "The tool has challenges with meaningful problems.", labels: ["Learner"] }, { text: "My tool should give me meaningful problems.", labels: ["Learner"] }, { text: "My problems should apply to real life.", labels: ["Learner"] }, { text: "AI tools should improve competency by increasing both intrinsic and extrinsic motivation within learners.", labels: ["Researcher"] }, { text: "I want AI to match learning to personal context.", labels: ["Learner", "Researcher"] }, { text: "I want the tool to offer hands-on on-demand practice of problem solving skills.", labels: ["Learner"] }],
            "i find it satisfying when the tool to gives me exactly what i need instantly and frustrating if it doesn't": [{ text: "I don't like lag time.", labels: ["Learner"] }, { text: "I want feedback to be responsive.", labels: ["Learner"] }, { text: "I want instant feedback.", labels: ["Learner"] }, { text: "I like it when the tool is available to gives me the answer I need quickly.", labels: ["Learner"] }, { text: "I feel frustrated when I can't get answers.", labels: ["Learner"] }, { text: "I like receiving immediate feedback.", labels: ["Learner"] }, { text: "I thought the AI was very helpful.", labels: ["Learner"] }],
            "i want the tool to recognize when i need help or have a misunderstanding and proactively provides me what i need": [{ text: "I want help when I'm stuck.", labels: ["Learner"] }, { text: "I want the AI to be proactive.", labels: ["Learner"] }, { text: "I want help with asking questions that give me the answers I want.", labels: ["Learner"] }, { text: "I want the tool to correct misconceptions.", labels: ["Researcher", "Developer"] }, { text: "The tool should provide immedate formative feedback.", labels: ["Researcher", "Developer"] }, { text: "I build tools that provide formative feedback in multiple modalities.", labels: ["Developer"] }],
            "i am concerned about the accuracy of the ai how do i know i can trust it and who is responsible when it is wrong": [{ text: "How do I know I can trust the accuracy of the tool's response?", labels: ["Learner"] }, { text: "I question SAMIs ability to match me with other students.", labels: ["Learner"] }, { text: "I am concerned about the accuracy of AI.", labels: ["Learner", "Instructor"] }, { text: "The tool doesn't understand synonyms or technical words.", labels: ["Learner"] }, { text: "Is the AI responsible for wrong answers?", labels: ["Learner", "Researcher"] }, { text: "There are some questions the AI can't answer.", labels: ["Learner"] }, { text: "I find it hard to be engaged if I cannot trust the AI.", labels: ["Learner"] }],
            "i want to minimize ai hallucination and prevent harmful output": [{ text: "The system has issues with mistakes/hallucinations.", labels: ["Developer", "Researcher"] }, { text: "I am concerned about toxic or inappropriate output from AI.", labels: ["Researcher", "Developer"] }],
            "i want to know who are using the tools who did what and to track their progress": [{ text: "I want to know if people are using the tools / doing the activities.", labels: ["Instructor"] }, { text: "I need a visualization dashboard for progress tracking and feedback.", labels: ["Developer", "Instructor"] }, { text: "I provide dashboards of student progress to teachers.", labels: ["Developer"] }, { text: "I want to know when my students are using the tool.", labels: ["Instructor"] }, { text: "I want to see detailed information about who did what.", labels: ["Instructor"] }],
            "i want detailed learning analytics through visualizations so i can make better instructional decisions": [{ text: "I want feedback to help instructors.", labels: ["Developer"] }, { text: "I want data do inform my classroom / instructional decision making.", labels: ["Instructor"] }, { text: "I want to see teacher dashboards optimized with clear visualizations that support better decision-making.", labels: ["Developer", "Instructor"] }, { text: "I want to increase teaching presence by giving instructors insight into learning data.", labels: ["Researcher", "Developer"] }, { text: "I want to collect data and make it available to support teachers/learners.", labels: ["Developer"] }, { text: "I need learning analytics that reveal meaningful insights via visualization / I want to see clear visualizations that make data actionable.", labels: ["Researcher", "Developer"] }],
            "i want the tools to collect data and use it to facilitate teacher and tool personalization": [{ text: "The tool uses data to personalize learning.", labels: ["Developer"] }, { text: "I provide data to personalize instruction.", labels: ["Developer"] }, { text: "I collect data to adapt the tool in real-time.", labels: ["Developer"] }, { text: "I need to facilitate personalization through visualization tools.", labels: ["Developer"] }, { text: "I want the data to highlight issues I can address.", labels: ["Instructor"] }, { text: "I want to know where students gets confused, and have problems and challenges.", labels: ["Instructor"] }, { text: "I need personalization that leverages real-time data and feedback loops to support timely adaption and continuous learning.", labels: ["Researcher", "Developer"] }],
            "learning tools should be designed for continuous improvements and learning engineering through historical data": [{ text: "My tools should be designed to facilitate learning engineering (learning about learning).", labels: ["Researcher"] }, { text: "I want to continuously improve the system by collecting detailed telemetry data for both researchers and instructors.", labels: ["Researcher", "Developer"] }, { text: "I want to improve the system by making inferences about data across semesters.", labels: ["Researcher", "Developer"] }, { text: "I want to utilize feedback loops.", labels: ["Researcher", "Developer"] }],
            "i want to build ai tools that are employ open standards to avoid vendor lock make third party integration easier and reduce cost": [{ text: "I avoid complexity / vendor lock related to tech deployment.", labels: ["Developer"] }, { text: "I prefer standards that make technology integration easier.", labels: ["Developer"] }, { text: "I want to build sustainable, generalizable, and cost-effective technology.", labels: ["Developer"] }],
            "i want ways to appropriately integrate ai with my other tools e g through lms integration and to effectively manage ai tool notifications": [{ text: "I want the tools available in my LMS.", labels: ["Instructor"] }, { text: "I think integrating with blackboard will make things easier for teachers and students.", labels: ["Instructor", "Developer"] }, { text: "I want more notifications from the tool.", labels: ["Learner"] }, { text: "I had trouble keeping track of the tool, including its notifications, where it is located, and how to use it.", labels: ["Learner"] }, { text: "I want to keep the AI separate from my regular learning tools.", labels: ["Instructor"] }],
            "the ai should be aligned with the content of my class": [{ text: "We should build AI tools that supplement existing course material.", labels: ["Developer"] }, { text: "I synchronize tool development to the class schedule to enable timely class support.", labels: ["Developer"] }, { text: "I want to support instructional design.", labels: ["Developer", "Researcher"] }, { text: "The tool should support class- relevant materials.", labels: ["Instructor"] }, { text: "The AI should use class data for increased relevance.", labels: ["Developer"] }, { text: "I want AI support learning core concepts.", labels: ["Instructor", "Developer"] }],
            "i won't use the ai if it isn't aligned with my teaching approach and style": [{ text: "working with non-familiar instructional styles can be confusing.", labels: ["Learner"] }, { text: "i prefer to use an instructional style that is familiar to me.", labels: ["Instructor"] }, { text: "I want to have agency in the tools used in my class.", labels: ["Instructor"] }, { text: "I (the teacher) need to be comfortable with the tool before I give it to my students.", labels: ["Instructor"] }, { text: "I want AI to match my teaching / instructional approach.", labels: ["Instructor"] }, { text: "I feel frustrated when the tool doesn't align with how I would teach this.", labels: ["Instructor"] }],
            "i want to involve teachers in the design process to increase relevance and alignment to course content and teach style towards increasing teaching presence": [{ text: "I want to involve stakeholders in the design process.", labels: ["Developer", "Researcher"] }, { text: "I want to design technology based on user needs.", labels: ["Developer"] }, { text: "I want the AI to be user-centered.", labels: ["Developer"] }, { text: "I want to increase teaching presence by improving the student experience in the learning tool.", labels: ["Researcher", "Developer"] }, { text: "I want to design tools that enhance teaching presence.", labels: ["Developer", "Researcher"] }],
            "i want ai to provide explanations and sources from human experts so i can believe what the ai says is correct": [{ text: "I want correct feedback.", labels: ["Learner"] }, { text: "I want my tool to mirror expert humans.", labels: ["Developer"] }, { text: "I believe providing explanations and sources will increase student trust.", labels: ["Researcher", "Developer"] }, { text: "I like when the tool links to additional info I can use to verify it and learn more.", labels: ["Learner"] }],
            "i prefer interacting with humans over ai and i don't want to lose the human part of learning": [{ text: "AI tools should enable human-human interaction.", labels: ["Developer", "Researcher"] }, { text: "I think being in person is important to my learning.", labels: ["Learner"] }, { text: "I prefer to work with a person rather than a robot.", labels: ["Learner"] }, { text: "I fear that AI tools will make learning less personal.", labels: ["Learner"] }],
            "ai could never replace my teacher": [{ text: "I prefer to connect with a teacher rather than using AI.", labels: ["Learner"] }, { text: "I don't think the AI does a good job of replacing the teacher's social interactions.", labels: ["Learner"] }, { text: "I think AI should be framed as an extension of the teacher not a replacement.", labels: ["Instructor", "Researcher"] }],
            "i am worried that ai interactions will degrade my social skills and relationships": [{ text: "I worry my social skills and the social skills of others will atrophy from using AI to help me engage socially.", labels: ["Learner"] }, { text: "Developing poor social skills from overusing technology.", labels: ["Learner", "Researcher"] }, { text: "I doubt AI can help me create meaningful social connections and might inhibit my future social learning.", labels: ["Learner"] }],
            "i find it intimidating to engage socially with others i don't want to annoy them or ask dumb questions": [{ text: "I find public forums intimidating to talk in.", labels: ["Learner"] }, { text: "I don't want to annoy my human instructors / tech designers.", labels: ["Learner"] }, { text: "I don't want to annoy my classmates and burn social capital.", labels: ["Learner"] }],
            "i don't know how to start a conversation with people i just met especially in large asynchronous classes or when there are barriers like large age generational differences": [{ text: "When I meet someone new, I am unsure of how to start conversation.", labels: ["Learner"] }, { text: "I want to meet people, but I feel like social introductions are a challenge.", labels: ["Learner"] }, { text: "I find social interaction challenging in large, online, and/or async classes.", labels: ["Learner"] }, { text: "I find it difficult to communicate across (age) generations.", labels: ["Learner"] }],
            "i want ai to foster community and a sense of belonging social presence": [{ text: "I want to design tools that increase social presence.", labels: ["Developer", "Researcher"] }, { text: "I want to build tools that foster a sense of belonging.", labels: ["Developer"] }, { text: "I think a sense of belonging is important.", labels: ["Learner", "Researcher"] }, { text: "I want to feel a sense of community.", labels: ["Learner"] }, { text: "We (AI tools) should build active community.", labels: ["Developer"] }, { text: "I don't want to be alone.", labels: ["Learner"] }],
            "my ai tools should help me to form deep social connections e g shared goals interests hobbies": [{ text: "I see the value of building social connections and think AI can help.", labels: ["Researcher", "Developer"] }, { text: "the tool has allowed me to create social connections I would have not made otherwise.", labels: ["Learner"] }, { text: "I want to make friends across a diverse cohort.", labels: ["Learner"] }, { text: "I want to connect with people who share my goals.", labels: ["Learner"] }, { text: "I want to build deep connections.", labels: ["Learner"] }, { text: "We (AI tools) should foster connection.", labels: ["Developer"] }, { text: "I want to connect with people near me, potentially in person.", labels: ["Learner"] }, { text: "I want to connect based on deeper features.", labels: ["Learner"] }],
        };

        function normKey(t) { return t.toLowerCase().replace(/[^\w\s']/g, ' ').replace(/\s+/g, ' ').trim(); }
        function findQuotes(ideation) {
            var k = normKey(ideation);
            if (quotesDB[k]) return quotesDB[k];
            var words = k.split(' '), best = null, bestScore = 0;
            Object.keys(quotesDB).forEach(function (dk) {
                var score = dk.split(' ').filter(function (w) { return words.indexOf(w) >= 0; }).length;
                if (score > bestScore) { bestScore = score; best = dk; }
            });
            return best ? quotesDB[best] : [];
        }

        var whoColors = {
            Instructor: { bg: '#e0f2fe', color: '#075985', border: '#bae6fd' },
            Learner: { bg: '#dcfce7', color: '#14532d', border: '#86efac' },
            Researcher: { bg: '#f3e8ff', color: '#4c1d95', border: '#d8b4fe' },
            Developer: { bg: '#fef3c7', color: '#78350f', border: '#fcd34d' }
        };

        function escHtml(s) {
            return String(s).replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;').replace(/\"/g, '&quot;');
        }

        function openSidebar(card) {
            var quotes = findQuotes(card.ideation);
            var ideation = card.ideation.replace(/\.$/, '') + '.';
            document.getElementById('sb-header').innerHTML =
                '<div style="display:flex;align-items:flex-start;justify-content:space-between;gap:12px">' +
                '<div>' +
                '<p style="font-size:.68rem;font-weight:700;letter-spacing:.08em;text-transform:uppercase;color:#6366f1;margin-bottom:4px">' + escHtml(card.guidelineCode) + ' &middot; Stakeholder Quotes</p>' +
                '<p style="font-size:.88rem;font-weight:700;color:#1e293b;line-height:1.4">' + escHtml(ideation) + '</p>' +
                '<p style="font-size:.72rem;color:#94a3b8;margin-top:6px">' + quotes.length + ' stakeholder quote' + (quotes.length !== 1 ? 's' : '') + ' from the field</p>' +
                '</div>' +
                '<button onclick="closeSidebar()" aria-label="Close" style="background:none;border:none;cursor:pointer;color:#64748b;padding:4px;border-radius:6px;flex-shrink:0" onmouseover="this.style.background=\'#f1f5f9\'" onmouseout="this.style.background=\'none\'">' +
                '<svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M18 6 6 18M6 6l12 12"/></svg>' +
                '</button></div>';
            var body = '';
            if (quotes.length === 0) {
                body = '<p style="color:#94a3b8;font-size:.85rem;font-style:italic">No stakeholder quotes found for this statement.</p>';
            } else {
                body = '<div style="display:flex;flex-direction:column;gap:10px">';
                quotes.forEach(function (entry) {
                    var badges = '';
                    if (entry.labels && entry.labels.length > 0) {
                        entry.labels.forEach(function (who) {
                            var wc = whoColors[who] || whoColors['Researcher'];
                            badges += '<span style="display:inline-block;font-size:.65rem;font-weight:700;letter-spacing:.05em;text-transform:uppercase;padding:2px 8px;border-radius:4px;margin-left:18px;margin-right:4px;background:' + wc.bg + ';color:' + wc.color + ';border:1px solid ' + wc.border + '">' + who + '</span>';
                        });
                    }
                    body += '<div style="background:#f8fafc;border-radius:8px;padding:12px 14px 10px;position:relative;border-left:3px solid #e0e7ff">' +
                        '<span style="font-family:Georgia,serif;font-size:2rem;line-height:.8;color:#c7d2fe;position:absolute;top:8px;left:10px;pointer-events:none">&ldquo;</span>' +
                        '<p style="font-size:.83rem;color:#1e293b;line-height:1.6;padding-left:18px;padding-top:4px;margin-bottom:8px">' + escHtml(entry.text) + '</p>' +
                        badges +
                        '</div>';
                });
                body += '</div>';
            }
            document.getElementById('sb-body').innerHTML = body;
            document.getElementById('sb-overlay').classList.add('sb-on');
            document.getElementById('sb-panel').classList.add('sb-open');
            document.body.style.overflow = 'hidden';
        }

        function closeSidebar() {
            document.getElementById('sb-overlay').classList.remove('sb-on');
            document.getElementById('sb-panel').classList.remove('sb-open');
            document.body.style.overflow = '';
            document.addEventListener('keydown', function (e) { if (e.key === 'Escape') closeSidebar(); });
        }
    </script>
    <script type="text/babel">
            const SingleSelectDropdown = ({ label, options, value, onChange }) => {
                const [isOpen, setIsOpen] = React.useState(false);
                const dropdownRef = React.useRef(null);

                React.useEffect(() => {
                    const handleClickOutside = (event) => {
                        if (dropdownRef.current && !dropdownRef.current.contains(event.target)) {
                            setIsOpen(false);
                        }
                    };
                    document.addEventListener("mousedown", handleClickOutside);
                    return () => document.removeEventListener("mousedown", handleClickOutside);
                }, [dropdownRef]);

                const handleSelect = (optionValue) => {
                    onChange(optionValue);
                    setIsOpen(false);
                };

                return (
                    <div className="relative" ref={dropdownRef}>
                        <button
                            onClick={() => setIsOpen(!isOpen)}
                            className="w-full h-11 px-4 text-left bg-white border border-gray-300 rounded-lg shadow-sm flex items-center justify-between focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent"
                        >
                            <span className="text-gray-700">{label}: <span
                                className="font-semibold">{options[value]}</span></span>
                            <svg className={`h-5 w-5 text-gray-400 transition-transform ${isOpen ? 'transform rotate-180' : ''}`}
                                xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                                <path fillRule="evenodd"
                                    d="M5.23 7.21a.75.75 0 011.06.02L10 11.168l3.71-3.938a.75.75 0 111.08 1.04l-4.25 4.5a.75.75 0 01-1.08 0l-4.25-4.5a.75.75 0 01.02-1.06z"
                                    clipRule="evenodd" />
                            </svg>
                        </button>
                        {isOpen && (
                            <div className="absolute z-20 w-full mt-2 bg-white border border-gray-200 rounded-lg shadow-xl overflow-y-auto">
                                <div className="p-2 space-y-1">
                                    {Object.entries(options).map(([optionValue, optionLabel]) => (
                                        <button
                                            key={optionValue}
                                            onClick={() => handleSelect(optionValue)}
                                            className={`w-full text-left px-3 py-2 text-sm rounded-md ${value === optionValue ? 'bg-indigo-500 text-white' : 'text-gray-800 hover:bg-gray-100'}`}
                                        >
                                            {optionLabel}
                                        </button>
                                    ))}
                                </div>
                            </div>
                        )}
                    </div>
                );
            };

            const MultiSelectDropdown = ({ category, label, options, selectedOptions, onFilterChange, onClear }) => {
                const [isOpen, setIsOpen] = React.useState(false);
                const dropdownRef = React.useRef(null);

                React.useEffect(() => {
                    const handleClickOutside = (event) => {
                        if (dropdownRef.current && !dropdownRef.current.contains(event.target)) {
                            setIsOpen(false);
                        }
                    };
                    document.addEventListener("mousedown", handleClickOutside);
                    return () => document.removeEventListener("mousedown", handleClickOutside);
                }, [dropdownRef]);

                const selectedCount = selectedOptions.size;

                return (
                    <div className="relative" ref={dropdownRef}>
                        <button
                            onClick={() => setIsOpen(!isOpen)}
                            className="w-full h-11 px-4 text-left bg-white border border-gray-300 rounded-lg shadow-sm flex items-center justify-between focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent"
                        >
                            <span className="text-gray-700">
                                {label} {selectedCount > 0 ? `(${selectedCount})` : ''}
                            </span>
                            <svg className={`h-5 w-5 text-gray-400 transition-transform ${isOpen ? 'transform rotate-180' : ''}`}
                                xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                                <path fillRule="evenodd"
                                    d="M5.23 7.21a.75.75 0 011.06.02L10 11.168l3.71-3.938a.75.75 0 111.08 1.04l-4.25 4.5a.75.75 0 01-1.08 0l-4.25-4.5a.75.75 0 01.02-1.06z"
                                    clipRule="evenodd" />
                            </svg>
                        </button>
                        {isOpen && (
                            <div className="absolute z-20 w-full mt-2 bg-white border border-gray-200 rounded-lg shadow-xl flex flex-col max-h-64">
                                <div className="p-4 space-y-3 overflow-y-auto max-h-52">
                                    {options.map(option => {
                                        const id = `filter-${category}-${option.replace(/\s+/g, '-')}`;
                                        return (
                                            <div key={id} className="flex items-center">
                                                <input
                                                    id={id}
                                                    type="checkbox"
                                                    checked={selectedOptions.has(option)}
                                                    onChange={(e) => onFilterChange(category, option, e.target.checked)}
                                                    className="custom-checkbox h-4 w-4 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500 cursor-pointer"
                                                />
                                                <label htmlFor={id}
                                                    className="ml-3 block text-sm text-gray-800 cursor-pointer select-none">{option}</label>
                                            </div>
                                        );
                                    })}
                                </div>
                                <div className="flex justify-between items-center px-4 py-2 border-t border-gray-100 bg-gray-50 rounded-b-lg">
                                    <span className="text-xs text-gray-500 font-medium">
                                        {selectedCount > 0 ? `${selectedCount} selected` : 'None selected'}
                                    </span>
                                    <button
                                        disabled={selectedCount === 0}
                                        onClick={(e) => {
                                            e.preventDefault();
                                            onClear();
                                        }}
                                        className={`text-xs font-bold transition-colors ${selectedCount > 0 ? 'text-indigo-600 hover:text-indigo-800' : 'text-gray-300 cursor-not-allowed'}`}
                                    >
                                        Clear
                                    </button>
                                </div>
                            </div>
                        )}
                    </div>
                );
            };

            const App = () => {
                const csvData = `Guideline Code,Associated Guideline,Ideation Issue Statement,COI dimension,Stakeholder,Problem
G1,AI tools should be open about data practices and share governance with learners,"I want to know what data is being collected about me and have control over how it will be used, so I can balance privacy with benefits",-,"Developer, Learner, Researcher","Access, Trust"
G2,AI tools should be accessible and fit into the busy lives of instructors and learners,My tools should adopt good design principles that make learning accessible to everyone,Cognitive,"Developer, Learner, Researcher",Access
G2,AI tools should be accessible and fit into the busy lives of instructors and learners,"I should be able to access AI help on a phone, with or without internet, and it should be affordable",Cognitive,"Developer, Instructor, Learner",Access
G2,AI tools should be accessible and fit into the busy lives of instructors and learners,"I (an adult) have limited time and resources, and learning needs to to fit into my life",Cognitive,"Instructor, Learner, Researcher",Access
G2,AI tools should be accessible and fit into the busy lives of instructors and learners,"I think AI's ability to provide extra help anytime anywhere makes it ""super human""",Cognitive,Learner,Access
G3,AI tools should be informed by learning science and learning theories,I think learning technologies should be informed by learning science and learning theories,Cognitive,"Developer, Researcher",Low Learning
G4,AI tools should be easy to understand and frictionless to use,The AI tool has a big learning curve and I need an introduction / tutorial on what it can do and how to use it,Cognitive,"Instructor, Learner","Low Engagement, Access"
G4,AI tools should be easy to understand and frictionless to use,"I need an AI tool that works without errors and is easy for me to understand quickly, or I won't use it (because my time is better spent elsewhere)",Cognitive,"Developer, Instructor, Learner","Low Engagement, Access"
G4,AI tools should be easy to understand and frictionless to use,"I want to design AI tools that are intuitive, easy to use, and promote natural interactions to avoid confusion over the interface and expectations of the tool",Cognitive,"Developer, Instructor, Learner, Researcher","Low Engagement, Access"
G5,AI tools should be transparent and explainable,I want to understand how the AI works and I want it to better understand me,Cognitive,Learner,Trust
G5,AI tools should be transparent and explainable,I want explanations for why my answers are wrong and/or how to do it,Cognitive,"Developer, Learner",Trust
G5,AI tools should be transparent and explainable,I want to know why the AI thinks my answer is wrong,Cognitive,"Developer, Learner, Researcher",Trust
G6,AI tools should minimize cognitive load by presenting essential information clearly and simply,I want to build tools that utilize design principles to direct attention and recall information where it is needed,Cognitive,"Developer, Learner","Low Learning, Low Engagement"
G6,AI tools should minimize cognitive load by presenting essential information clearly and simply,"I want tools that are simple and do not unnecessarily increase cognitive load, because I believe it will motivate students to use the tool more",Cognitive,"Developer, Researcher","Low Learning, Low Engagement"
G7,AI tools should provide learners with agency over their learning,"I want to options/choices for how I engage with content, as well as access to supplementary content",Cognitive,"Instructor, Learner","Low Adoption, Low Engagement"
G7,AI tools should provide learners with agency over their learning,I want to have agency in my learning,Cognitive,"Developer, Instructor, Learner, Researcher","Low Adoption, Low Engagement"
G8,AI tools should cultivate learners' metacognitive and critical thinking skills,"I want to build tools that coach meta-cognitive strategies, such as summarization, pre-writing, self-explanation, hypothesis testing",Cognitive,"Developer, Learner, Researcher",Low Learning
G8,AI tools should cultivate learners' metacognitive and critical thinking skills,I want my AI tools to cultivate reflective and critical thinking skills,Cognitive,"Developer, Instructor, Researcher",Low Learning
G9,AI tools should support learners' motivation and cultivate self-efficacy,"I think engagement improves learning, so we use several techniques to drive AI tool engagement (motivational messages/feedback, social connections, personalization, competition, and intrinsic/extrinsic rewards)",Cognitive,"Developer, Learner, Researcher",Low Engagement
G9,AI tools should support learners' motivation and cultivate self-efficacy,"I want to view my self as successful, so progress motivates me and being told I'm wrong feels terrible",Cognitive,"Developer, Instructor, Learner, Researcher",Low Engagement
G9,AI tools should support learners' motivation and cultivate self-efficacy,I want tools that scaffold problem solving step-by-step,Cognitive,"Developer, Learner",Low Engagement
G10,"AI tools should encourage active, constructive, and interactive engagement over passive consumption",I want tools that engage students in interactive inquiry/questioning through a conversational/discussion-based chatbot,Cognitive,"Developer, Instructor, Learner, Researcher","Low Learning, Low Engagement"
G10,"AI tools should encourage active, constructive, and interactive engagement over passive consumption",I want to collaborate with my peers to improve my learning,Cognitive,"Developer, Instructor, Learner, Researcher","Low Learning, Low Engagement"
G10,"AI tools should encourage active, constructive, and interactive engagement over passive consumption",I design generative tasks that require learners to restructure knowledge and create their own understanding,Cognitive,"Developer, Instructor, Researcher","Low Learning, Low Engagement"
G10,"AI tools should encourage active, constructive, and interactive engagement over passive consumption",I structure learning experiences to promote active and interactive engagement over passive consumption,Cognitive,"Developer, Researcher","Low Learning, Low Engagement"
G11,"AI tools should personalize learning based on learners' knowledge, skills, and abilities","I want to build tools that personalize to learner knowledge, adapting the learning sequence and level of difficulty","Cognitive, Teaching","Developer, Learner, Researcher",Low Learning
G11,"AI tools should personalize learning based on learners' knowledge, skills, and abilities","Adults have varied knowledge, expertise, and needs, and personalization is more important for adults learners than K12 learners","Cognitive, Teaching",Researcher,Low Learning
G12,"AI tools should offer meaningful challenges that are aligned with learners' context, goals, and career",I want my learning to support my career,"Cognitive, Teaching",Learner,Low Engagement
G12,"AI tools should offer meaningful challenges that are aligned with learners' context, goals, and career",I find learning more engaging when it is aligned to my goals,"Cognitive, Teaching","Developer, Learner, Researcher",Low Engagement
G12,"AI tools should offer meaningful challenges that are aligned with learners' context, goals, and career",I want to practice meaningful problems relevant to my context,"Cognitive, Teaching","Learner, Researcher",Low Engagement
G13,"AI tools should provide accurate, contextualized help when learners need it","I find it satisfying when the tool to gives me exactly what  I need instantly, and frustrating if it doesn't","Teaching, Cognitive",Learner,"Low Engagement, Trust, Low Learning"
G13,"AI tools should provide accurate, contextualized help when learners need it",I want the tool to recognize when I need help or have a misunderstanding and proactively provides me what I need,"Teaching, Cognitive","Developer, Learner, Researcher","Low Engagement, Trust, Low Learning"
G13,"AI tools should provide accurate, contextualized help when learners need it",I am concerned about the accuracy of the AI—how do I know I can trust it and who is responsible when it is wrong?,"Teaching, Cognitive","Instructor, Learner, Researcher","Low Engagement, Trust, Low Learning"
G13,"AI tools should provide accurate, contextualized help when learners need it",I want to minimize AI hallucination and prevent harmful output,"Teaching, Cognitive","Developer, Researcher","Low Engagement, Trust, Low Learning"
G14,"AI tools should provide actionable insights to instructors, learners, and researchers","I want to know who are using the tools, who did what, and to track their progress","Teaching, Cognitive","Developer, Instructor","Low Engagement, Low Learning"
G14,"AI tools should provide actionable insights to instructors, learners, and researchers",I want detailed learning analytics (through visualizations) so I can make better instructional decisions,"Teaching, Cognitive","Developer, Instructor, Researcher","Low Engagement, Low Learning"
G14,"AI tools should provide actionable insights to instructors, learners, and researchers",I want the tools to collect data and use it to facilitate teacher and tool personalization,"Teaching, Cognitive","Developer, Instructor, Researcher","Low Engagement, Low Learning"
G14,"AI tools should provide actionable insights to instructors, learners, and researchers",Learning tools should be designed for continuous improvements and learning engineering through historical data,"Teaching, Cognitive","Developer, Researcher","Low Engagement, Low Learning"
G15,AI tools should integrate with users' existing educational technology ecosystem,"I want to build AI tools that are employ open standards to avoid vendor lock, make third-party integration easier, and reduce cost",Teaching,Developer,Low Adoption
G15,AI tools should integrate with users' existing educational technology ecosystem,"I want ways to appropriately integrate AI with my other tools (eg, through LMS integration) and to effectively manage AI tool notifications",Teaching,"Developer, Instructor, Learner",Low Adoption
G16,AI tools should align with instructional practices and course content,The AI should be aligned with the content of my class,Teaching,"Developer, Instructor, Researcher","Low Adoption, Trust"
G16,AI tools should align with instructional practices and course content,I won't use the AI if it isn't aligned with my teaching approach and style,Teaching,"Instructor, Learner","Low Adoption, Trust"
G16,AI tools should align with instructional practices and course content,I want to involve teachers in the design process to increase relevance and alignment to course content and teach style towards increasing teaching presence,Teaching,"Developer, Researcher","Low Adoption, Trust"
G17,AI tools should maintain the human touch and supplement rather than replace human instructors,I want AI to provide explanations and sources from human experts so I can believe what the AI says is correct,"Social, Teaching","Developer, Learner, Researcher","Low Adoption, Trust"
G17,AI tools should maintain the human touch and supplement rather than replace human instructors,"I prefer interacting with humans (over AI) and I don't want to lose the ""human"" part of learning","Social, Teaching","Developer, Learner, Researcher","Low Adoption, Trust"
G17,AI tools should maintain the human touch and supplement rather than replace human instructors,I want AI to provide explanations and sources from human experts so I can believe what the AI says is correct,"Social, Teaching","Developer, Learner, Researcher","Low Adoption, Trust"
G17,AI tools should maintain the human touch and supplement rather than replace human instructors,"I prefer interacting with humans (over AI) and I don't want to lose the ""human"" part of learning","Social, Teaching","Developer, Learner, Researcher","Low Adoption, Trust"
G17,AI tools should maintain the human touch and supplement rather than replace human instructors,AI could never replace my teacher,"Social, Teaching","Instructor, Learner, Researcher","Low Adoption, Trust"
G18,AI tools should scaffold and support learners in developing their social competencies,I am worried that AI interactions will degrade my social skills and relationships,"Social, Teaching","Learner, Researcher",Low Engagement
G18,AI tools should scaffold and support learners in developing their social competencies,I find it intimidating to engage socially with others—I don't want to annoy them or ask dumb questions,"Social, Teaching",Learner,Low Engagement
G18,AI tools should scaffold and support learners in developing their social competencies,"I don't know how to start a conversation with people I just met, especially in large asynchronous classes or when there are barriers like large age/generational differences","Social, Teaching",Learner,Low Engagement
G19,AI tools should foster social connection and community,I want AI to foster community and a sense of belonging,Social,"Developer, Learner, Researcher","Low Adoption, Low Engagement"
G19,AI tools should foster social connection and community,"My AI tools should help me to form deep social connections (eg, shared goals, interests, hobbies)",Social,"Developer, Learner, Researcher","Low Adoption, Low Engagement"`;

                const [searchTerm, setSearchTerm] = React.useState('');
                const [searchOn, setSearchOn] = React.useState('both');
                const [groupBy, setGroupBy] = React.useState('guideline');
                const [filters, setFilters] = React.useState({ stakeholder: new Set(), problem: new Set(), coi: new Set() });
                const [filtersExpanded, setFiltersExpanded] = React.useState(true);

                const cardsData = React.useMemo(() => {
                    const rows = csvData.trim().split('\n');
                    const headers = rows.shift().split(',').map(h => h.trim());
                    return rows.map((row, index) => {
                        const values = row.match(/(".*?"|[^",]+)(?=\s*,|\s*$)/g) || [];
                        const card = { id: index };
                        headers.forEach((header, i) => {
                            let value = (values[i] || '').trim().replace(/^"|"$/g, '').replace('Reseacher', 'Researcher');
                            let key = '';
                            if (header.toLowerCase().includes('guideline code')) key = 'guidelineCode';
                            else if (header.toLowerCase().includes('guideline')) key = 'guideline';
                            else if (header.toLowerCase().includes('ideation')) key = 'ideation';
                            else if (header.toLowerCase().includes('coi')) key = 'coi';
                            else if (header.toLowerCase().includes('stakeholder')) key = 'stakeholder';
                            else if (header.toLowerCase().includes('problem')) key = 'problem';

                            if (key) {
                                if (['coi', 'stakeholder', 'problem'].includes(key)) {
                                    card[key] = value.split(',').map(s => s.trim()).filter(Boolean);
                                } else {
                                    card[key] = value;
                                }
                            }
                        });
                        return card;
                    });
                }, []);

                const uniqueTags = React.useMemo(() => ({
                    stakeholder: [...new Set(cardsData.flatMap(card => card.stakeholder || []))].sort(),
                    problem: [...new Set(cardsData.flatMap(card => card.problem || []))].sort(),
                    coi: [...new Set(cardsData.flatMap(card => card.coi || []))].sort(),
                }), [cardsData]);

                const filteredAndGroupedCards = React.useMemo(() => {
                    const filteredCards = cardsData.filter(card => {
                        const term = searchTerm.trim();
                        let matchesSearch = true;

                        if (term) {
                            const isPhraseSearch = term.startsWith('"') && term.endsWith('"');

                            const ideationText = card.ideation.toLowerCase();
                            const guidelineText = card.guideline.toLowerCase();

                            if (isPhraseSearch) {
                                const phrase = term.substring(1, term.length - 1).toLowerCase();
                                if (phrase) {
                                    if (searchOn === 'ideation') {
                                        matchesSearch = ideationText.includes(phrase);
                                    } else if (searchOn === 'guideline') {
                                        matchesSearch = guidelineText.includes(phrase);
                                    } else {
                                        matchesSearch = ideationText.includes(phrase) || guidelineText.includes(phrase);
                                    }
                                }
                            } else {
                                const keywords = term.toLowerCase().split(' ').filter(kw => kw.length > 0);

                                const checkText = (text) => keywords.every(kw => text.includes(kw));

                                if (searchOn === 'ideation') {
                                    matchesSearch = checkText(ideationText);
                                } else if (searchOn === 'guideline') {
                                    matchesSearch = checkText(guidelineText);
                                } else {
                                    const combinedText = ideationText + ' ' + guidelineText;
                                    matchesSearch = checkText(combinedText);
                                }
                            }
                        }

                        if (!matchesSearch) return false;

                        for (const category in filters) {
                            const activeTags = filters[category];
                            if (activeTags.size > 0 && !([...activeTags].some(tag => card[category].includes(tag)))) {
                                return false;
                            }
                        }
                        return true;
                    });

                    const groups = {};
                    filteredCards.forEach(card => {
                        let groupKeys = (groupBy === 'guideline') ? [`${card.guidelineCode}: ${card.guideline.replace(/\.$/, '')}`] : (card[groupBy] || ['Uncategorized']);
                        groupKeys.forEach(key => {
                            if (!groups[key]) groups[key] = [];
                            groups[key].push(card);
                        });
                    });

                    const sortedGroupKeys = Object.keys(groups).sort((a, b) => {
                        if (groupBy === 'guideline') {
                            const matchA = a.match(/^G(\d+):/);
                            const matchB = b.match(/^G(\d+):/);
                            if (matchA && matchB) {
                                const numA = parseInt(matchA[1], 10);
                                const numB = parseInt(matchB[1], 10);
                                return numA - numB;
                            }
                        }
                        return a.localeCompare(b);
                    });

                    return sortedGroupKeys.map(key => ({
                        groupKey: key,
                        cards: groups[key]
                    }));

                }, [searchTerm, searchOn, filters, groupBy, cardsData]);

                const handleFilterChange = (category, value, isChecked) => {
                    setFilters(prevFilters => {
                        const newSet = new Set(prevFilters[category]);
                        if (isChecked) newSet.add(value);
                        else newSet.delete(value);
                        return { ...prevFilters, [category]: newSet };
                    });
                };

                const clearAllFilters = () => {
                    setFilters({ stakeholder: new Set(), problem: new Set(), coi: new Set() });
                };

                const handleTagClick = (category, value) => {
                    setFilters(prevFilters => {
                        const newSet = new Set(prevFilters[category]);
                        if (newSet.has(value)) newSet.delete(value);
                        else newSet.add(value);
                        return { ...prevFilters, [category]: newSet };
                    });
                    if (!filtersExpanded) setFiltersExpanded(true);
                };

                const activeFilterCount = filters.stakeholder.size + filters.problem.size + filters.coi.size;


                const tagColors = {
                    stakeholder: 'bg-sky-100 text-sky-800 border-sky-200',
                    problem: 'bg-rose-100 text-rose-800 border-rose-200',
                    coi: 'bg-amber-100 text-amber-800 border-amber-200'
                };

                const lightbulbIcon = <svg xmlns="http://www.w3.org/2000/svg" className="h-6 w-6 mr-2 text-yellow-400"
                    viewBox="0 0 20 20" fill="currentColor">
                    <path d="M11 3a1 1 0 10-2 0v1a1 1 0 102 0V3zM15.657 5.757a1 1 0 00-1.414-1.414l-.707.707a1 1 0 001.414 1.414l.707-.707zM9 12a1 1 0 012 0v5a1 1 0 11-2 0v-5zM4.343 5.757a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414l.707.707zM1 10a1 1 0 011-1h1a1 1 0 110 2H2a1 1 0 01-1-1z" />
                    <path d="M10 7a3 3 0 00-3 3c0 1.657 1.343 3 3 3s3-1.343 3-3a3 3 0 00-3-3z" />
                    <path d="M12.586 14.586A2 2 0 119.414 11.414a2 2 0 013.172 3.172z" />
                    <path d="M16.95 11a1 1 0 100-2h-1a1 1 0 100 2h1z" />
                </svg>;

                const groupByOptions = {
                    guideline: 'Guideline',
                    stakeholder: 'Stakeholder',
                    problem: 'Problem',
                    coi: 'COI Dimension',
                };

                return (
                    <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative">
                        <header className="text-center pt-12 md:pt-16 pb-4">
                            <h1 className="text-4xl md:text-5xl font-extrabold text-gray-900 tracking-tight">AI Technology Design Guidelines Explorer</h1>
                            <p className="mt-3 max-w-2xl mx-auto text-lg text-gray-500">An interactive search tool to explore design guidelines
                                for AI technologies that support Adult Learners.</p>
                            <div className="mt-5 flex justify-center">
                                <a
                                    href="https://doi.org/10.1145/3800645.3813102"
                                    target="_blank"
                                    rel="noopener noreferrer"
                                    className="flex items-center gap-2 px-3.5 py-2 text-sm font-semibold text-gray-600 hover:text-indigo-600 bg-white hover:bg-indigo-50/30 border border-gray-200 hover:border-indigo-200 rounded-lg shadow-sm hover:shadow transition-all duration-200"
                                >
                                    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2" strokeLinecap="round" strokeLinejoin="round" className="flex-shrink-0">
                                        <path d="M14.5 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V7.5L14.5 2z" />
                                        <polyline points="14 2 14 8 20 8" />
                                        <line x1="16" y1="13" x2="8" y2="13" />
                                        <line x1="16" y1="17" x2="8" y2="17" />
                                        <line x1="10" y1="9" x2="8" y2="9" />
                                    </svg>
                                    <span>Link to Paper</span>
                                </a>
                            </div>
                        </header>
                        <div className="control-panel rounded-xl border border-gray-200 bg-white/50">
                            <div className="flex justify-between items-center px-5 py-3">
                                <button
                                    onClick={() => setFiltersExpanded(!filtersExpanded)}
                                    className="flex items-center gap-2 text-sm font-semibold text-gray-700 hover:text-indigo-600 transition-colors focus:outline-none"
                                    aria-expanded={filtersExpanded}
                                    aria-controls="panel-content"
                                >
                                    <svg
                                        className={`collapse-toggle-icon h-5 w-5 ${filtersExpanded ? 'rotated' : ''}`}
                                        xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor"
                                    >
                                        <path fillRule="evenodd" d="M5.23 7.21a.75.75 0 011.06.02L10 11.168l3.71-3.938a.75.75 0 111.08 1.04l-4.25 4.5a.75.75 0 01-1.08 0l-4.25-4.5a.75.75 0 01.02-1.06z" clipRule="evenodd" />
                                    </svg>
                                    <span>Search &amp; Filters</span>
                                    {activeFilterCount > 0 && <span className="inline-flex items-center justify-center min-w-5 h-5 px-1.5 text-xs font-bold text-white bg-indigo-500 rounded-full">{activeFilterCount} active</span>}
                                    {searchTerm && <span className="inline-flex items-center px-2 py-0.5 text-xs font-medium text-emerald-700 bg-emerald-100 rounded-full border border-emerald-200">searching</span>}
                                </button>
                                {activeFilterCount > 0 && (
                                    <button onClick={clearAllFilters}
                                        className="text-sm font-semibold text-indigo-600 hover:text-indigo-800 transition-colors">Clear
                                        All Filters
                                    </button>
                                )}
                            </div>

                            <div id="panel-content" className={`filter-collapse ${filtersExpanded ? 'expanded' : ''}`}>
                                <div className="px-5 pb-5 space-y-4">
                                    <div className="grid grid-cols-1 md:grid-cols-5 gap-4 items-center">
                                        <div className="md:col-span-3">
                                            <label htmlFor="search" className="block text-sm font-semibold text-gray-700 mb-2">Search
                                                Guidelines</label>
                                            <div className="relative">
                                                <div className="pointer-events-none absolute inset-y-0 left-0 pl-3 flex items-center">
                                                    <svg className="h-5 w-5 text-gray-400" xmlns="http://www.w3.org/2000/svg"
                                                        viewBox="0 0 20 20" fill="currentColor" aria-hidden="true">
                                                        <path fillRule="evenodd"
                                                            d="M9 3.5a5.5 5.5 0 100 11 5.5 5.5 0 000-11zM2 9a7 7 0 1112.452 4.391l3.328 3.329a.75.75 0 11-1.06 1.06l-3.329-3.328A7 7 0 012 9z"
                                                            clipRule="evenodd" />
                                                    </svg>
                                                </div>
                                                <input
                                                    type="text"
                                                    id="search"
                                                    placeholder={`Keywords or "exact phrase"`}
                                                    value={searchTerm}
                                                    onChange={(e) => setSearchTerm(e.target.value)}
                                                    className="block w-full rounded-lg border-gray-300 shadow-sm pl-10 h-11 focus:border-indigo-500 focus:ring-indigo-500"
                                                />
                                            </div>
                                        </div>
                                        <div className="md:col-span-2 self-end">
                                            <div id="search-on" className="inline-flex rounded-md shadow-sm w-full h-11" role="group">
                                                {['both', 'ideation', 'guideline'].map((item) => (
                                                    <button
                                                        key={item}
                                                        type="button"
                                                        onClick={() => setSearchOn(item)}
                                                        className={`w-full px-4 py-2 text-sm font-medium capitalize border first:rounded-l-lg last:rounded-r-lg ${searchOn === item ? 'bg-indigo-600 text-white border-indigo-600 z-10' : 'bg-white text-gray-700 border-gray-300 hover:bg-gray-50'}`}
                                                    >
                                                        {item === 'both' ? 'All Text' : item}
                                                    </button>
                                                ))}
                                            </div>
                                        </div>
                                    </div>

                                    <div>
                                        <div className="flex justify-between items-center mb-2 mt-2">
                                            <h3 className="text-sm font-semibold text-gray-700">Filters</h3>
                                        </div>
                                        <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 items-center">
                                            <SingleSelectDropdown label="Group By" options={groupByOptions} value={groupBy}
                                                onChange={setGroupBy} />
                                            <MultiSelectDropdown category="stakeholder" label="Stakeholders"
                                                options={uniqueTags.stakeholder}
                                                selectedOptions={filters.stakeholder}
                                                onFilterChange={handleFilterChange}
                                                onClear={() => setFilters(prev => ({ ...prev, stakeholder: new Set() }))} />
                                            <MultiSelectDropdown category="problem" label="Problems"
                                                options={uniqueTags.problem}
                                                selectedOptions={filters.problem}
                                                onFilterChange={handleFilterChange}
                                                onClear={() => setFilters(prev => ({ ...prev, problem: new Set() }))} />
                                            <MultiSelectDropdown category="coi" label="COI Dimensions"
                                                options={uniqueTags.coi}
                                                selectedOptions={filters.coi}
                                                onFilterChange={handleFilterChange}
                                                onClear={() => setFilters(prev => ({ ...prev, coi: new Set() }))} />
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <main id="card-groups-container" className="space-y-12 py-10">
                            {filteredAndGroupedCards.length > 0 ? (
                                filteredAndGroupedCards.map(({ groupKey, cards }) => (
                                    <div key={groupKey}>
                                        <h2 className="text-2xl md:text-3xl font-bold text-gray-800 mb-6 pb-3 border-b-2 border-indigo-200">{groupKey}</h2>
                                        <div className="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-8">
                                            {cards.map(card => (
                                                <div key={card.id}
                                                    className="flex flex-col bg-gradient-to-br from-indigo-50 via-white to-sky-100 rounded-xl shadow-lg border border-gray-200/80 overflow-hidden transform hover:-translate-y-1 transition-transform duration-300">
                                                    <div className="p-4 sm:p-5 bg-gray-800 text-white flex items-center justify-between gap-3">
                                                        <div className="flex items-center flex-shrink-0">
                                                            {lightbulbIcon}
                                                            <h3 className="font-bold whitespace-nowrap">Ideation Statement</h3>
                                                        </div>
                                                        <button onClick={() => openSidebar(card)} className="flex items-center gap-1.5 text-xs font-semibold text-gray-300 hover:text-white hover:bg-gray-700 px-2 py-1 rounded-lg transition-colors border border-transparent flex-shrink-0 ml-auto whitespace-nowrap">
                                                            <svg className="flex-shrink-0" width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5"><path d="M21 15a2 2 0 0 1-2 2H7l-4 4V5a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2z" /></svg>
                                                            <span>Stakeholder Quotes</span>
                                                        </button>
                                                    </div>
                                                    <div className="p-6 flex-grow"><p
                                                        className="text-gray-600 text-lg italic leading-relaxed">"{card.ideation}"</p>
                                                    </div>
                                                    <div className="px-6 pb-6 space-y-4 mt-auto">
                                                        <div className="border-t border-gray-200 pt-4 space-y-3">
                                                            {['stakeholder', 'problem', 'coi'].map(cat => (
                                                                card[cat] && card[cat].length > 0 && (
                                                                    <div key={cat}
                                                                        className="flex items-start">
                                                                        <p className="text-xs font-bold text-gray-500 w-28 flex-shrink-0">{cat.charAt(0).toUpperCase() + cat.slice(1)}:</p>
                                                                        <div className="flex flex-wrap gap-2">{card[cat].map(tag => {
                                                                            const isActive = filters[cat].has(tag);
                                                                            return <span key={tag}
                                                                                onClick={() => handleTagClick(cat, tag)}
                                                                                className={`tag-clickable inline-block px-3 py-1 text-xs font-semibold rounded-full border ${tagColors[cat]} ${isActive ? 'active-filter' : ''}`}
                                                                                title={`Click to ${isActive ? 'remove' : 'filter by'} ${tag}`}
                                                                                role="button"
                                                                                tabIndex={0}
                                                                                onKeyDown={(e) => { if (e.key === 'Enter' || e.key === ' ') handleTagClick(cat, tag); }}
                                                                            >{tag}</span>;
                                                                        })}</div>
                                                                    </div>
                                                                )
                                                            ))}
                                                        </div>
                                                        <div className="border-t border-gray-200 pt-4"><p
                                                            className="text-sm font-semibold text-indigo-800">{card.guidelineCode}: {card.guideline.replace(/\.$/, '')}</p>
                                                        </div>

                                                    </div>
                                                </div>
                                            ))}
                                        </div>
                                    </div>
                                ))
                            ) : (
                                <div className="text-center py-16 px-6 bg-white rounded-lg shadow-sm border">
                                    <h3 className="text-xl font-semibold text-gray-700">No guidelines match your
                                        criteria.</h3>
                                    <p className="mt-2 text-gray-500">Try adjusting your search or clearing some
                                        filters.</p>
                                </div>
                            )}
                        </main>
                    </div>
                );
            };

            ReactDOM.createRoot(document.getElementById('root')).render(<App />);
    </script>

    <div id="sb-overlay" onclick="closeSidebar()"></div>
    <aside id="sb-panel" role="dialog" aria-modal="true" aria-label="Stakeholder quotes">
        <div id="sb-header"></div>
        <div id="sb-body"></div>
    </aside>
</body>

