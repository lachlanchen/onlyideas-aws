<template>
  <div class="landing">
    <header class="hero">
      <div class="container">
        <nav class="nav">
          <a class="logo" href="#">OnlyIdeas</a>
          <div class="nav-links">
            <a href="#philosophy">Philosophy</a>
            <a href="#process">How it works</a>
            <a href="#faq">FAQ</a>
            <a class="nav-cta" href="#join">Join waitlist</a>
          </div>
        </nav>
        <div class="hero-content">
          <div class="hero-copy">
            <p class="eyebrow">Idea-first creation</p>
            <h1>The lightest lift launch platform for people who only have ideas.</h1>
            <p class="lead">
              You bring the spark, we assemble the product, team, and revenue engine.
              OnlyIdeas lets you stay in the ideation seat while your concepts turn into cash-generating products.
            </p>
            <div class="hero-actions">
              <a class="cta" href="#join">Start with an idea</a>
              <a class="secondary" href="#process">See how it works</a>
            </div>
            <div class="metrics">
              <div class="metric">
                <span class="metric-value">500+</span>
                <span class="metric-label">Concepts validated</span>
              </div>
              <div class="metric">
                <span class="metric-value">38%</span>
                <span class="metric-label">Average revenue share secured for creators</span>
              </div>
              <div class="metric">
                <span class="metric-value">24 hrs</span>
                <span class="metric-label">From idea intake to action plan</span>
              </div>
            </div>
          </div>
          <div class="hero-card">
            <h2>The OnlyIdeas canvas</h2>
            <p>
              A single-page blueprint that captures the promise of your idea and routes it to the right builders, backers, and beta users.
            </p>
            <ul>
              <li>
                <span class="card-label">You bring</span>
                <span class="card-copy">A one-line concept and the audience it empowers.</span>
              </li>
              <li>
                <span class="card-label">We deliver</span>
                <span class="card-copy">Research, prototypes, launch partners, and a predictable go-to-market path.</span>
              </li>
              <li>
                <span class="card-label">You earn</span>
                <span class="card-copy">Transparent revenue shares while ideating the next big thing.</span>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </header>

    <main>
      <section class="philosophy" id="philosophy">
        <div class="container">
          <div class="section-header">
            <p class="eyebrow">Why OnlyIdeas</p>
            <h2>Your idea becomes the business.</h2>
            <p class="section-lead">
              We designed OnlyIdeas for creators who want to stay focused on imagination, story, and direction.
              You define the vision, we build the engine that makes it real and profitable.
            </p>
          </div>
          <div class="highlight-grid">
            <article
              v-for="highlight in highlights"
              :key="highlight.title"
              class="highlight-card"
            >
              <h3>{{ highlight.title }}</h3>
              <p>{{ highlight.description }}</p>
            </article>
          </div>
        </div>
      </section>

      <section class="process" id="process">
        <div class="container">
          <div class="section-header">
            <p class="eyebrow">How it works</p>
            <h2>From spark to shared revenue in three beats.</h2>
          </div>
          <div class="process-grid">
            <article
              v-for="(step, index) in steps"
              :key="step.title"
              class="process-step"
            >
              <span class="step-number">0{{ index + 1 }}</span>
              <h3>{{ step.title }}</h3>
              <p>{{ step.description }}</p>
            </article>
          </div>
        </div>
      </section>

      <section class="principles">
        <div class="container">
          <div class="principle-grid">
            <article
              v-for="principle in principles"
              :key="principle.title"
              class="principle-card"
            >
              <h3>{{ principle.title }}</h3>
              <p>{{ principle.description }}</p>
            </article>
          </div>
        </div>
      </section>

      <section class="social-proof">
        <div class="container">
          <div class="section-header">
            <p class="eyebrow">Believers</p>
            <h2>Creators who only had an idea.</h2>
          </div>
          <div class="testimonial-grid">
            <figure
              v-for="testimonial in testimonials"
              :key="testimonial.author"
              class="testimonial-card"
            >
              <blockquote>“{{ testimonial.quote }}”</blockquote>
              <figcaption>
                <span class="author">{{ testimonial.author }}</span>
                <span class="role">{{ testimonial.role }}</span>
              </figcaption>
            </figure>
          </div>
        </div>
      </section>

      <section class="faq" id="faq">
        <div class="container">
          <div class="section-header">
            <p class="eyebrow">FAQ</p>
            <h2>Still wondering how ideas become income?</h2>
            <p class="section-lead">
              We remove the heavy lift of strategy, build, and launch so you can stay in flow.
              Here is what creators ask before partnering.
            </p>
          </div>
          <div class="faq-list">
            <article
              v-for="faq in faqs"
              :key="faq.question"
              class="faq-item"
            >
              <h3>{{ faq.question }}</h3>
              <p>{{ faq.answer }}</p>
            </article>
          </div>
        </div>
      </section>

      <section class="callout" id="join">
        <div class="container">
          <h2>Bring your next idea to life the light way.</h2>
          <p>
            Share your concept, claim your upside, and let OnlyIdeas orchestrate the rest.
            We are building a studio where creativity is the only capital you need.
          </p>
          <div class="callout-actions">
            <a class="cta" href="mailto:hello@onlyideas.art">Share an idea</a>
            <a class="secondary" href="#faq">Need more detail?</a>
          </div>
        </div>
      </section>
    </main>

    <footer class="footer">
      <div class="container">
        <span class="logo">OnlyIdeas</span>
        <p>Dream in ideas. Profit from imagination.</p>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
const highlights = [
  {
    title: 'Idea-first by design',
    description:
      'OnlyIdeas exists for founders who lead with imagination. Hand us the concept and we assemble the technology, talent, and growth engine around it.'
  },
  {
    title: 'Transparent upside',
    description:
      'We align incentives from day one with clear revenue sharing agreements, milestone notifications, and dashboards that show exactly how your idea converts.'
  },
  {
    title: 'Creation on autopilot',
    description:
      'Product research, prototypes, launch campaigns, customer support. Our studio handles the execution so you can keep ideating.'
  },
  {
    title: 'Momentum that compounds',
    description:
      'As your ideas launch, you unlock a private community of builders and investors ready to accelerate the next one.'
  }
];

const steps = [
  {
    title: 'Share the spark',
    description:
      'Submit your hypothesis and the audience you want to empower. We capture the promise, plot the opportunity, and schedule a clarity session.'
  },
  {
    title: 'Co-design the blueprint',
    description:
      'In less than a day we map milestones, resourcing, and revenue models. You approve the plan, we deploy the studio team to build.'
  },
  {
    title: 'Launch and profit',
    description:
      'We ship, market, and operate the product under your story. Earnings flow transparently to your dashboard while you develop the next concept.'
  }
];

const principles = [
  {
    title: 'Ideas over assets',
    description:
      'We back the thinkers, not the feature list. OnlyIdeas champions the insight that sparks change and protects your creative equity.'
  },
  {
    title: 'Lightweight collaboration',
    description:
      'Everything lives inside a shared workspace so you can review strategy, prototypes, and performance without managing the team.'
  },
  {
    title: 'Aligned ambition',
    description:
      'Our success is tied to your upside. We invest resources where the idea is boldest and share wins every step of the journey.'
  }
];

const testimonials = [
  {
    quote:
      'I arrived with a napkin sketch. Two sprints later we had paying subscribers and a revenue share that keeps compounding.',
    author: 'Mia Chen',
    role: 'Creator, Future Folio'
  },
  {
    quote:
      'OnlyIdeas let me stay focused on trend spotting while they built the product, the brand, and the investor deck.',
    author: 'Luis Romero',
    role: 'Strategist, Idea Atlas'
  }
];

const faqs = [
  {
    question: 'Do I need a prototype before joining?',
    answer:
      'No. A clear idea, the audience you want to empower, and the change you want to see are enough. We translate that into the roadmap.'
  },
  {
    question: 'How are earnings shared?',
    answer:
      'We agree on transparent revenue percentages up front. As the product sells, your dashboard shows live earnings and upcoming payouts.'
  },
  {
    question: 'What happens to ownership of my idea?',
    answer:
      'You retain the story and directional rights. OnlyIdeas partners on execution, amplifies the reach, and aligns incentives so you benefit long term.'
  }
];
</script>

<style scoped>
.landing {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  color: #1f2532;
}

.container {
  width: min(1100px, 92vw);
  margin: 0 auto;
}

.hero {
  padding: 3.5rem 0 5rem;
  background: linear-gradient(120deg, rgba(255, 245, 231, 0.8), rgba(246, 248, 255, 0.95) 55%, #ffffff);
  position: relative;
}

.nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 2rem;
  margin-bottom: 4rem;
}

.logo {
  font-weight: 700;
  font-size: 1.5rem;
  letter-spacing: 0.08em;
}

.nav-links {
  display: flex;
  align-items: center;
  gap: 1.5rem;
  font-size: 0.95rem;
}

.nav-links a {
  color: inherit;
}

.nav-cta {
  padding: 0.6rem 1.2rem;
  border-radius: 999px;
  background: #283cf5;
  color: #ffffff;
  font-weight: 600;
}

.hero-content {
  display: grid;
  gap: 3rem;
  align-items: center;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
}

.hero-copy h1 {
  font-size: clamp(2.5rem, 4vw, 3.5rem);
  line-height: 1.1;
  margin: 0.8rem 0;
}

.eyebrow {
  text-transform: uppercase;
  letter-spacing: 0.18em;
  font-size: 0.75rem;
  font-weight: 600;
  color: rgba(31, 37, 50, 0.7);
}

.lead {
  font-size: 1.05rem;
  max-width: 540px;
  margin: 1.2rem 0 2rem;
  color: rgba(31, 37, 50, 0.8);
}

.hero-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  margin-bottom: 2rem;
}

.cta,
.secondary {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0.9rem 1.6rem;
  border-radius: 999px;
  font-weight: 600;
  transition: transform 0.25s ease, box-shadow 0.25s ease, background 0.25s ease;
}

.cta {
  background: #283cf5;
  color: #ffffff;
  box-shadow: 0 12px 24px rgba(40, 60, 245, 0.25);
}

.cta:hover {
  transform: translateY(-2px);
  box-shadow: 0 16px 32px rgba(40, 60, 245, 0.22);
}

.secondary {
  background: rgba(255, 255, 255, 0.7);
  color: #283cf5;
  border: 1px solid rgba(40, 60, 245, 0.2);
}

.secondary:hover {
  background: rgba(40, 60, 245, 0.08);
}

.metrics {
  display: grid;
  gap: 1.5rem;
  grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
}

.metric {
  display: flex;
  flex-direction: column;
  gap: 0.35rem;
}

.metric-value {
  font-size: 1.6rem;
  font-weight: 700;
  color: #283cf5;
}

.metric-label {
  font-size: 0.9rem;
  color: rgba(31, 37, 50, 0.65);
}

.hero-card {
  background: #ffffff;
  border-radius: 1.4rem;
  padding: 2rem;
  box-shadow: 0 20px 45px rgba(31, 37, 50, 0.08);
  border: 1px solid rgba(40, 60, 245, 0.08);
}

.hero-card h2 {
  margin: 0 0 0.75rem;
}

.hero-card p {
  margin: 0 0 1.5rem;
  color: rgba(31, 37, 50, 0.7);
}

.hero-card ul {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.hero-card li {
  display: grid;
  gap: 0.3rem;
}

.card-label {
  font-size: 0.75rem;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: rgba(31, 37, 50, 0.52);
}

.card-copy {
  font-size: 0.95rem;
  color: rgba(31, 37, 50, 0.8);
}

main {
  display: flex;
  flex-direction: column;
  gap: 4rem;
  padding: 4rem 0 5rem;
}

.section-header {
  max-width: 600px;
  margin-bottom: 2.5rem;
}

.section-header h2 {
  margin: 0.4rem 0;
  font-size: clamp(2rem, 3vw, 2.6rem);
}

.section-lead {
  margin: 0;
  color: rgba(31, 37, 50, 0.75);
}

.highlight-grid {
  display: grid;
  gap: 1.5rem;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
}

.highlight-card {
  background: #ffffff;
  padding: 1.8rem;
  border-radius: 1.2rem;
  box-shadow: 0 14px 30px rgba(31, 37, 50, 0.06);
  border: 1px solid rgba(40, 60, 245, 0.05);
}

.highlight-card h3 {
  margin: 0 0 0.75rem;
  font-size: 1.2rem;
}

.highlight-card p {
  margin: 0;
  color: rgba(31, 37, 50, 0.7);
}

.process-grid {
  display: grid;
  gap: 1.5rem;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
}

.process-step {
  background: #ffffff;
  border-radius: 1.2rem;
  padding: 2rem;
  box-shadow: 0 12px 26px rgba(31, 37, 50, 0.06);
  border: 1px solid rgba(40, 60, 245, 0.04);
}

.step-number {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background: rgba(40, 60, 245, 0.12);
  color: #283cf5;
  font-weight: 700;
  margin-bottom: 1rem;
}

.process-step h3 {
  margin: 0 0 0.6rem;
}

.process-step p {
  margin: 0;
  color: rgba(31, 37, 50, 0.7);
}

.principle-grid {
  display: grid;
  gap: 1.2rem;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
}

.principle-card {
  padding: 1.8rem;
  border-radius: 1.1rem;
  border: 1px solid rgba(40, 60, 245, 0.08);
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(8px);
}

.principle-card h3 {
  margin: 0 0 0.7rem;
}

.principle-card p {
  margin: 0;
  color: rgba(31, 37, 50, 0.7);
}

.social-proof {
  background: linear-gradient(135deg, rgba(247, 250, 255, 0.7), rgba(255, 248, 236, 0.8));
  padding: 4rem 0;
}

.testimonial-grid {
  display: grid;
  gap: 1.5rem;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
}

.testimonial-card {
  background: #ffffff;
  border-radius: 1.2rem;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  box-shadow: 0 18px 36px rgba(31, 37, 50, 0.07);
  border: 1px solid rgba(40, 60, 245, 0.05);
}

.testimonial-card blockquote {
  margin: 0;
  font-size: 1.05rem;
  line-height: 1.6;
  color: rgba(31, 37, 50, 0.82);
}

.testimonial-card figcaption {
  display: flex;
  flex-direction: column;
  gap: 0.2rem;
  font-size: 0.85rem;
  color: rgba(31, 37, 50, 0.6);
}

.testimonial-card .author {
  font-weight: 600;
  color: #1f2532;
}

.faq-list {
  display: grid;
  gap: 1.2rem;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
}

.faq-item {
  background: #ffffff;
  padding: 1.8rem;
  border-radius: 1.1rem;
  box-shadow: 0 12px 28px rgba(31, 37, 50, 0.05);
  border: 1px solid rgba(40, 60, 245, 0.04);
}

.faq-item h3 {
  margin: 0 0 0.7rem;
}

.faq-item p {
  margin: 0;
  color: rgba(31, 37, 50, 0.72);
}

.callout {
  background: linear-gradient(120deg, rgba(40, 60, 245, 0.12), rgba(255, 202, 134, 0.18));
  padding: 4rem 0;
  text-align: center;
}

.callout h2 {
  margin: 0 0 1rem;
  font-size: clamp(2rem, 3.5vw, 2.8rem);
}

.callout p {
  margin: 0 auto 2rem;
  max-width: 520px;
  color: rgba(31, 37, 50, 0.75);
}

.callout-actions {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
  gap: 1rem;
}

.footer {
  border-top: 1px solid rgba(31, 37, 50, 0.08);
  padding: 2rem 0;
  background: #ffffff;
}

.footer .container {
  display: flex;
  flex-direction: column;
  gap: 0.7rem;
  text-align: center;
  color: rgba(31, 37, 50, 0.68);
}

@media (max-width: 768px) {
  .nav {
    flex-direction: column;
    align-items: flex-start;
    margin-bottom: 2.5rem;
  }

  .nav-links {
    flex-wrap: wrap;
    justify-content: flex-start;
  }

  .hero {
    padding-top: 2.5rem;
  }

  .hero-actions {
    width: 100%;
  }

  .hero-card {
    padding: 1.6rem;
  }

  main {
    padding-top: 3rem;
  }
}
</style>
