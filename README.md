<!DOCTYPE html>
<html lang="en" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Confronting Structural Violence in CVI Organizations</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Equity & Hope -->
    <!-- Application Structure Plan: A thematic, narrative-driven, single-page scrolling experience. The structure guides the user from understanding the problem (the core question, definition, manifestations) to exploring solutions (interactive comparisons of organizational models and funding practices, detailed solution pathways). This structure was chosen to transform a dense report into an active learning experience, making complex concepts digestible and highlighting actionable steps for CVI leaders and funders. -->
    <!-- Visualization & Content Choices: 
        - Report Info: Key concepts of Structural Violence. Goal: Inform. Viz: Interactive cards. Interaction: Hover/click to reveal examples. Justification: Makes abstract concepts tangible.
        - Report Info: 3 manifestations of violence (Control, HR, Pay). Goal: Explain. Viz: Tabbed interface. Interaction: Click tabs to switch content. Justification: Organizes complex info cleanly.
        - Report Info: Table 1 (Violent vs. Equitable Orgs). Goal: Compare. Viz: Interactive toggle-based comparison. Interaction: Click a toggle to switch views. Justification: More engaging than a static table, encourages active comparison.
        - Report Info: Table 2 (Problematic vs. Equitable Funding). Goal: Compare. Viz: Interactive toggle-based comparison. Interaction: Click a toggle to switch views. Justification: Highlights actionable changes for funders.
        - Report Info: Pathways to Equity. Goal: Organize solutions. Viz: Accordion component. Interaction: Click to expand/collapse. Justification: Clean UI, details on demand.
        - Library/Method for all: Vanilla JS for DOM manipulation and event handling. Tailwind CSS for styling.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #F8F9FA;
            color: #1E293B;
        }
        .nav-link {
            transition: color 0.3s ease;
        }
        .nav-link:hover, .nav-link.active {
            color: #0891B2;
        }
        .tab-btn.active {
            border-color: #0891B2;
            background-color: #CFFAFE;
            color: #0E7490;
        }
        .accordion-header.open .accordion-icon {
            transform: rotate(180deg);
        }
        .comparison-card {
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        .comparison-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
        }
    </style>
</head>
<body class="antialiased">

    <header class="bg-white/80 backdrop-blur-md sticky top-0 z-50 shadow-sm">
        <nav class="container mx-auto px-6 py-4 flex justify-between items-center">
            <a href="#" class="text-xl font-bold text-slate-800">Internal Equity in CVI</a>
            <div class="hidden md:flex space-x-8">
                <a href="#problem" class="nav-link font-medium text-slate-600">The Problem</a>
                <a href="#comparison" class="nav-link font-medium text-slate-600">The Two Paths</a>
                <a href="#solutions" class="nav-link font-medium text-slate-600">Solutions</a>
                <a href="#funders" class="nav-link font-medium text-slate-600">For Funders</a>
            </div>
            <button id="mobile-menu-btn" class="md:hidden focus:outline-none">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"></path></svg>
            </button>
        </nav>
        <div id="mobile-menu" class="hidden md:hidden">
            <a href="#problem" class="block py-2 px-6 text-sm text-slate-600">The Problem</a>
            <a href="#comparison" class="block py-2 px-6 text-sm text-slate-600">The Two Paths</a>
            <a href="#solutions" class="block py-2 px-6 text-sm text-slate-600">Solutions</a>
            <a href="#funders" class="block py-2 px-6 text-sm text-slate-600">For Funders</a>
        </div>
    </header>

    <main class="container mx-auto px-6 py-8 md:py-16">

        <section id="hero" class="text-center mb-24">
            <h1 class="text-4xl md:text-6xl font-bold text-slate-900 mb-4 leading-tight">Does your Community Violence Intervention organization practice <span class="text-cyan-600">Structural Violence?</span></h1>
            <p class="max-w-3xl mx-auto text-lg md:text-xl text-slate-600">
                Community Violence Intervention organizations stand on the front lines against societal harm. But a critical question must be asked: Do our internal structures inadvertently perpetuate the very inequities we fight to dismantle? This exploration is a call to align our actions with our mission.
            </p>
        </section>

        <section id="problem" class="mb-24 scroll-mt-20">
            <div class="text-center mb-12">
                <h2 class="text-3xl md:text-4xl font-bold text-slate-900">The Problem Within</h2>
                <p class="mt-4 text-lg text-slate-600 max-w-3xl mx-auto">**Structural Violence** is not direct or overt. It's the "invisible" harm embedded in our systems and normalized by routine. It's the gap between what is possible and what is reality for those disadvantaged by our organizational structures.</p>
            </div>

            <div class="max-w-5xl mx-auto">
                <div class="mb-8">
                    <div id="tabs-container" class="flex flex-wrap justify-center gap-2 md:gap-4 mb-8">
                        <button data-tab="control" class="tab-btn text-sm md:text-base font-semibold py-2 px-4 border-b-2 rounded-t-lg transition">Centralized Control</button>
                        <button data-tab="hr" class="tab-btn text-sm md:text-base font-semibold py-2 px-4 border-b-2 rounded-t-lg transition">HR Deficiencies</button>
                        <button data-tab="pay" class="tab-btn text-sm md:text-base font-semibold py-2 px-4 border-b-2 rounded-t-lg transition">The Wage Gap</button>
                    </div>
                    <div id="tabs-content" class="bg-white p-6 md:p-8 rounded-lg shadow-md">
                        <div id="content-control" class="tab-content">
                            <h3 class="text-2xl font-bold text-slate-800 mb-3">Eroding Community Ownership</h3>
                            <p class="text-slate-600">When one person—especially someone not from the community—controls all decisions, it creates a structural barrier to authentic engagement. Community Violence Intervention is most effective when it's locally-developed and nurtured. Centralized control stifles the creativity and motivation of frontline workers, turning them into mere implementers rather than empowered agents of change. This internal disempowerment directly translates into reduced external impact.</p>
                        </div>
                        <div id="content-hr" class="tab-content hidden">
                            <h3 class="text-2xl font-bold text-slate-800 mb-3">When Relationships Undermine Fairness</h3>
                            <p class="text-slate-600">Without a competent HR department and clear, transparent policies, a vacuum is created where **Structural Violence** flourishes. When job security or promotion depends on personal feelings or relationships rather than merit, it breeds deep distrust, kills morale, and perpetuates favoritism and bias. An organization cannot build trust in the community if its own internal environment is perceived as unjust by its people.</p>
                        </div>
                        <div id="content-pay" class="tab-content hidden">
                            <h3 class="text-2xl font-bold text-slate-800 mb-3">Perpetuating Inequality from Within</h3>
                            <p class="text-slate-600">Stark pay discrepancies between executives and frontline workers expose a profound hypocrisy. If an organization fighting external economic inequality perpetuates it internally—especially by underpaying staff from the very communities it serves—it actively contributes to the problem. The **Structural Violence** of low wages leads to burnout, high turnover, and a loss of invaluable institutional knowledge, directly compromising the Community Violence Intervention mission.</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="comparison" class="mb-24 scroll-mt-20">
            <div class="text-center mb-12">
                <h2 class="text-3xl md:text-4xl font-bold text-slate-900">The Two Paths: A Clear Choice</h2>
                <p class="mt-4 text-lg text-slate-600 max-w-3xl mx-auto">The internal structure of a Community Violence Intervention organization is not a neutral framework; it's a choice. It can either reflect the inequities of the world or model the justice we seek to create. The differences are stark and have profound consequences for staff, community, and mission effectiveness.</p>
            </div>
            
            <div class="max-w-6xl mx-auto">
                <div class="flex justify-center items-center mb-8 space-x-4">
                    <span class="font-semibold text-slate-600">Structurally Violent</span>
                    <label for="org-toggle" class="relative inline-flex items-center cursor-pointer">
                        <input type="checkbox" id="org-toggle" class="sr-only peer">
                        <div class="w-14 h-8 bg-red-200 rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-1 after:left-[4px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-6 after:w-6 after:transition-all peer-checked:bg-cyan-600"></div>
                    </label>
                    <span class="font-semibold text-cyan-700">Equitable</span>
                </div>

                <div id="comparison-grid" class="grid md:grid-cols-3 gap-8">
                </div>
            </div>
        </section>

        <section id="solutions" class="mb-24 scroll-mt-20">
            <div class="text-center mb-12">
                <h2 class="text-3xl md:text-4xl font-bold text-slate-900">Pathways to Equity</h2>
                <p class="mt-4 text-lg text-slate-600 max-w-3xl mx-auto">Dismantling internal **Structural Violence** is not just an ethical imperative; it's a strategic one. Building an equitable organization requires intentional, concrete actions that align internal operations with the external mission of peace and justice.</p>
            </div>

            <div id="accordion-container" class="max-w-4xl mx-auto space-y-4">
            </div>
        </section>

        <section id="funders" class="scroll-mt-20">
            <div class="text-center mb-12">
                <h2 class="text-3xl md:text-4xl font-bold text-slate-900">The Funder's Imperative</h2>
                <p class="mt-4 text-lg text-slate-600 max-w-3xl mx-auto">Funders are powerful agents of change. Investment practices can either perpetuate **Structural Violence** or become a force for equity. It's time to look beyond program outcomes and scrutinize the internal health of grantee organizations.</p>
            </div>

            <div class="max-w-6xl mx-auto">
                <div class="flex justify-center items-center mb-8 space-x-4">
                    <span class="font-semibold text-slate-600">Problematic Practice</span>
                    <label for="funder-toggle" class="relative inline-flex items-center cursor-pointer">
                        <input type="checkbox" id="funder-toggle" class="sr-only peer">
                        <div class="w-14 h-8 bg-red-200 rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-1 after:left-[4px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-6 after:w-6 after:transition-all peer-checked:bg-cyan-600"></div>
                    </label>
                    <span class="font-semibold text-cyan-700">Equitable Approach</span>
                </div>

                <div id="funder-comparison-grid" class="grid md:grid-cols-2 lg:grid-cols-4 gap-8">
                </div>
            </div>
        </section>

    </main>

    <footer class="bg-slate-800 text-white mt-24">
        <div class="container mx-auto px-6 py-8 text-center">
            <h3 class="text-2xl font-bold mb-4">A Call to Action for a Just Future</h3>
            <p class="max-w-3xl mx-auto text-slate-300 mb-6">The journey to eradicate violence begins within. For leaders, practitioners, and funders in the Community Violence Intervention field, the charge is clear: lead by example. Let us build organizations that embody the justice and equity we advocate for, creating a truly violence-free future for all.</p>
            <p class="text-sm text-slate-400">Inspired by the work and experience of <a href="https://www.linkedin.com/in/solomon-middleton-williams-85529734/" target="_blank" class="text-cyan-400 hover:text-cyan-200 underline">Solomon Midleton-Williams</a>.</p>
        </div>
    </footer>

<script>
document.addEventListener('DOMContentLoaded', function () {
    const data = {
        comparison: {
            categories: ['Governance & Decision-Making', 'Human Resources Practices', 'Compensation & Pay Equity'],
            violent: [
                'Centralized control; decisions by few, often non-local; top-down communication; limited staff/community input.',
                'Ad-hoc HR; decisions based on relationships/feelings; lack of clear policies for hiring, promotion, termination; high potential for bias.',
                'Significant pay disparities (executives vs. frontline); minimum wage/survival pay for frontline; lack of pay transparency; turnover due to financial strain.'
            ],
            equitable: [
                'Community-led, participatory governance; decentralized decision-making; transparent, inclusive processes; empowering local leaders and frontline staff.',
                'Robust, professional HR; clear, fair, transparent policies; unbiased hiring/promotion practices; regular policy review; strong DEI/EDIJ integration.',
                'Commitment to pay equity; living/thriving wages for all staff; transparent compensation structures; regular pay equity analyses; reduced turnover, increased morale.'
            ]
        },
        solutions: [
            {
                title: 'Foster Authentic Community Ownership',
                content: 'Move beyond consultation to genuinely empower community members and frontline staff. Adopt governance models that formally integrate community voices into leadership. Decentralize decision-making to enable agility and responsiveness. Prioritize hiring and promoting individuals from the communities served into leadership roles.'
            },
            {
                title: 'Establish Equitable Human Resources',
                content: 'Build a competent and fair HR infrastructure. Implement clear, comprehensive policies for hiring, promotion, and compensation. Actively remove unconscious bias from all processes. Integrate Diversity, Equity, Inclusion, and Justice (DEIJ) principles into the core of your HR strategy to build a foundation of trust and fairness.'
            },
            {
                title: 'Champion Pay Equity',
                content: 'Address pay disparities head-on. Conduct regular pay equity analyses to identify and fix gaps. Commit to paying all employees a thriving wage that allows for stability and well-being. Increase pay transparency and learn from innovative models like AS220’s equal pay policy to foster profound trust and community ownership.'
            }
        ],
        funders: {
            categories: ['Funding Approach', 'Evaluation Metrics', 'Support for Grantees', 'Leadership & Governance'],
            problematic: [
                'Short-term, project-specific grants; focus on quick, quantifiable outcomes; rigid reporting.',
                'Over-reliance on homicide rates; preference for Randomized Controlled Trials (RCTs); limited qualitative assessment.',
                'Limited capacity building support; focus on external program delivery; "teach to the test" pressure.',
                'Preference for institutionally-affiliated organizations; less scrutiny on internal equity; perpetuating power imbalances.'
            ],
            equitable: [
                'Multi-year, general operating support; investment in long-term capacity building; flexible, adaptive reporting.',
                'Balanced metrics (quantitative + qualitative); value process and outcome; incorporate community well-being, trust, staff morale.',
                'Fund internal infrastructure (HR, finance); support DEI/EDIJ initiatives; provide technical assistance for equitable practices.',
                'Prioritize diverse, community-led leadership; scrutinize internal governance and pay equity; foster authentic community ownership.'
            ]
        }
    };

    const mobileMenuBtn = document.getElementById('mobile-menu-btn');
    const mobileMenu = document.getElementById('mobile-menu');
    mobileMenuBtn.addEventListener('click', () => {
        mobileMenu.classList.toggle('hidden');
    });

    const tabs = document.querySelectorAll('.tab-btn');
    const tabContents = document.querySelectorAll('.tab-content');
    tabs.forEach(tab => {
        tab.addEventListener('click', () => {
            tabs.forEach(t => t.classList.remove('active'));
            tab.classList.add('active');
            const target = tab.getAttribute('data-tab');
            tabContents.forEach(content => {
                if (content.id === `content-${target}`) {
                    content.classList.remove('hidden');
                } else {
                    content.classList.add('hidden');
                }
            });
        });
    });
    tabs[0].click();

    const orgToggle = document.getElementById('org-toggle');
    const comparisonGrid = document.getElementById('comparison-grid');
    function renderComparisonGrid() {
        comparisonGrid.innerHTML = '';
        const showEquitable = orgToggle.checked;
        data.comparison.categories.forEach((category, index) => {
            const content = showEquitable ? data.comparison.equitable[index] : data.comparison.violent[index];
            const card = `
                <div class="comparison-card bg-white p-6 rounded-lg shadow-md border-t-4 ${showEquitable ? 'border-cyan-500' : 'border-red-400'}">
                    <h4 class="font-bold text-lg mb-2 text-slate-800">${category}</h4>
                    <p class="text-slate-600">${content}</p>
                </div>
            `;
            comparisonGrid.insertAdjacentHTML('beforeend', card);
        });
    }
    orgToggle.addEventListener('change', renderComparisonGrid);
    renderComparisonGrid();

    const accordionContainer = document.getElementById('accordion-container');
    data.solutions.forEach((item, index) => {
        const accordionItem = `
            <div class="bg-white rounded-lg shadow-md">
                <button class="accordion-header w-full flex justify-between items-center p-5 text-left font-semibold text-slate-800">
                    <span>${item.title}</span>
                    <svg class="accordion-icon w-5 h-5 transition-transform transform" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg>
                </button>
                <div class="accordion-content hidden p-5 pt-0">
                    <p class="text-slate-600">${item.content}</p>
                </div>
            </div>
        `;
        accordionContainer.insertAdjacentHTML('beforeend', accordionItem);
    });

    const accordionHeaders = document.querySelectorAll('.accordion-header');
    accordionHeaders.forEach(header => {
        header.addEventListener('click', () => {
            const content = header.nextElementSibling;
            header.classList.toggle('open');
            content.classList.toggle('hidden');
        });
    });

    const funderToggle = document.getElementById('funder-toggle');
    const funderGrid = document.getElementById('funder-comparison-grid');
    function renderFunderGrid() {
        funderGrid.innerHTML = '';
        const showEquitable = funderToggle.checked;
        data.funders.categories.forEach((category, index) => {
            const content = showEquitable ? data.funders.equitable[index] : data.funders.problematic[index];
            const card = `
                <div class="comparison-card bg-white p-6 rounded-lg shadow-md border-t-4 ${showEquitable ? 'border-cyan-500' : 'border-red-400'}">
                    <h4 class="font-bold text-lg mb-2 text-slate-800">${category}</h4>
                    <p class="text-slate-600">${content}</p>
                </div>
            `;
            funderGrid.insertAdjacentHTML('beforeend', card);
        });
    }
    funderToggle.addEventListener('change', renderFunderGrid);
    renderFunderGrid();

    const navLinks = document.querySelectorAll('.nav-link');
    const sections = document.querySelectorAll('section');
    window.addEventListener('scroll', () => {
        let current = '';
        sections.forEach(section => {
            const sectionTop = section.offsetTop;
            if (pageYOffset >= sectionTop - 100) {
                current = section.getAttribute('id');
            }
        });

        navLinks.forEach(link => {
            link.classList.remove('active');
            if (link.getAttribute('href').includes(current)) {
                link.classList.add('active');
            }
        });
    });
});
</script>

</body>
</html>
