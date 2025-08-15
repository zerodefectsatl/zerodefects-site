import React, { useEffect, useMemo, useState } from "react";

// Zero Defects ATL — AI‑Optimized, Local SEO + AEO/GEO‑ready SPA
// Notes for Chris:
// 1) Replace placeholders like PHONE_NUMBER, BUSINESS_EMAIL, STREET_ADDRESS, and MAP_EMBED_URL.
// 2) Swap Unsplash demo images with your real project photos to boost trust and conversions.
// 3) When we deploy, we'll add analytics (GA4), Search Console, and connect to your Google Business Profile UTM links.

const PHONE_NUMBER = "(404) 406‑3355"; // TODO: replace
const PHONE_TEL = "+14044063355"; // TODO: replace with E.164 format e.g. +16785551234
const BUSINESS_EMAIL = "zerodefectsatl@gmail.com"; // TODO: replace
const CANONICAL_URL = "https://www.zerodefectsatl.com"; // keep if domain remains
const BUSINESS_NAME = "Zero Defects ATL";
const CITY = "Braselton";
const REGION = "GA";
const POSTAL = "30517";
const SERVICE_AREAS = [
  "Braselton",
  "Buford",
  "Hoschton",
  "Flowery Branch",
  "Gainesville",
  "Dacula",
  "Suwanee",
  "Winder",
  "Hamilton Mill"
];

// Simple stars icon
const Stars = () => (
  <div className="flex gap-1" aria-label="5 star rating">
    {Array.from({ length: 5 }).map((_, i) => (
      <svg key={i} viewBox="0 0 24 24" className="w-5 h-5" fill="currentColor" aria-hidden>
        <path d="M12 .587l3.668 7.431 8.2 1.192-5.934 5.786 1.402 8.172L12 18.896l-7.336 3.872 1.402-8.172L.132 9.21l8.2-1.192z" />
      </svg>
    ))}
  </div>
);

const Badge = ({ children }) => (
  <span className="inline-flex items-center gap-2 rounded-full border border-white/20 bg-white/10 px-3 py-1 text-white/90 text-sm backdrop-blur">
    {children}
  </span>
);

const Section = ({ id, title, kicker, children, className = "" }) => (
  <section id={id} className={`max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-16 ${className}`}>
    <div className="mb-10">
      {kicker && <p className="text-sm tracking-widest uppercase text-primary/70 font-semibold">{kicker}</p>}
      <h2 className="text-3xl md:text-4xl font-extrabold tracking-tight">{title}</h2>
    </div>
    {children}
  </section>
);

export default function Site() {
  const [mounted, setMounted] = useState(false);
  useEffect(() => setMounted(true), []);

  // ---- HEAD meta + OG tags injection ----
  useEffect(() => {
    document.title = `${BUSINESS_NAME} | Ceramic Coating • Paint Correction • PPF in ${CITY}, ${REGION}`;

    const ensure = (tagName, attrs) => {
      const selector = Object.entries(attrs)
        .map(([k, v]) => `[${k}="${v}"]`)
        .join("");
      let el = document.head.querySelector(`${tagName}${selector}`);
      if (!el) {
        el = document.createElement(tagName);
        Object.entries(attrs).forEach(([k, v]) => el.setAttribute(k, v));
        document.head.appendChild(el);
      }
      return el;
    };

    ensure("meta", { name: "description", content: `Premium ceramic coatings, paint correction, and paint protection film (PPF) for ${CITY}, ${REGION} and surrounding areas. Zero Defects ATL delivers show‑car gloss with pro‑grade products and meticulous prep.` });
    ensure("link", { rel: "canonical", href: CANONICAL_URL });

    ensure("meta", { property: "og:title", content: `${BUSINESS_NAME} — Detailing in ${CITY}, ${REGION}` });
    ensure("meta", { property: "og:description", content: `Ceramic coatings • Paint correction • PPF in ${CITY}. Trusted by enthusiasts near Road Atlanta.` });
    ensure("meta", { property: "og:url", content: CANONICAL_URL });
    ensure("meta", { property: "og:type", content: "website" });
    ensure("meta", { name: "robots", content: "index,follow" });
  }, []);

  // ---- JSON‑LD (LocalBusiness, Services, FAQ) ----
  const ldLocalBusiness = useMemo(() => ({
    "@context": "https://schema.org",
    "@type": "AutoDetailing",
    name: BUSINESS_NAME,
    url: CANONICAL_URL,
    telephone: PHONE_NUMBER,
    email: BUSINESS_EMAIL,
    address: {
      "@type": "PostalAddress",
      addressLocality: CITY,
      addressRegion: REGION,
      postalCode: POSTAL,
      addressCountry: "US"
      // address: "STREET_ADDRESS" // optional street address
    },
    areaServed: SERVICE_AREAS,
    image: [
      "https://images.unsplash.com/photo-1542362567-b07e54358753",
      "https://images.unsplash.com/photo-1525609004556-c46c7d6cf023",
      "https://images.unsplash.com/photo-1550355291-bbee04a92027"
    ],
    openingHoursSpecification: [
      {
        "@type": "OpeningHoursSpecification",
        dayOfWeek: ["Monday","Tuesday","Wednesday","Thursday","Friday"],
        opens: "09:00",
        closes: "18:00"
      }
    ],
    sameAs: [
      // "https://www.facebook.com/yourpage",
      // "https://www.instagram.com/yourhandle",
      // "https://g.page/your-gbp"
    ]
  }), []);

  const ldServices = useMemo(() => ({
    "@context": "https://schema.org",
    "@graph": [
      {
        "@type": "Service",
        name: "Ceramic Coating Service",
        serviceType: "Ceramic Coating",
        provider: { "@type": "Organization", name: BUSINESS_NAME },
        areaServed: SERVICE_AREAS,
        description: "Long‑term protection and gloss with professional prep, decontamination, and application."
      },
      {
        "@type": "Service",
        name: "Paint Correction Service",
        serviceType: "Paint Correction",
        provider: { "@type": "Organization", name: BUSINESS_NAME },
        areaServed: SERVICE_AREAS,
        description: "Multi‑stage correction to remove swirls, haze, and light defects for mirror finish."
      },
      {
        "@type": "Service",
        name: "Paint Protection Film (PPF)",
        serviceType: "Paint Protection Film",
        provider: { "@type": "Organization", name: BUSINESS_NAME },
        areaServed: SERVICE_AREAS,
        description: "Self‑healing film installed on high‑impact areas (front clip, rockers, etc.)."
      }
    ]
  }), []);

  const faqs = [
    {
      q: "What does ceramic coating do?",
      a: "It creates a hard, glossy, hydrophobic layer over your clear coat that resists UV, chemicals, and light marring—making washing faster and the finish deeper."
    },
    {
      q: "How long does a coating last?",
      a: "With proper maintenance, pro‑grade coatings can last 2–5+ years. We’ll recommend options based on your driving and storage."
    },
    { q: "Do I still need to wash the car?", a: "Yes. Coatings aren’t force fields, but they keep contaminants from sticking and make safe washing much easier." },
    { q: "What is paint correction?", a: "Machine polishing that levels micro‑defects to restore clarity and gloss before protection is applied." },
    { q: "Is PPF better than ceramic?", a: "They solve different problems. PPF protects against rock chips and impacts; coatings add gloss, slickness, and chemical resistance. Many customers do both." },
    { q: "Do you offer mobile service?", a: "For most correction and coatings we work in‑shop for best results. Maintenance details may be mobile—ask for availability in your area." }
  ];

  const ldFAQ = useMemo(() => ({
    "@context": "https://schema.org",
    "@type": "FAQPage",
    mainEntity: faqs.map(({ q, a }) => ({
      "@type": "Question",
      name: q,
      acceptedAnswer: { "@type": "Answer", text: a }
    }))
  }), []);

  const mailtoQuote = (data) => {
    const subject = encodeURIComponent("Quote Request — Zero Defects ATL");
    const body = encodeURIComponent(
      [
        `Name: ${data.name}`,
        `Email: ${data.email}`,
        `Phone: ${data.phone}`,
        `Service: ${data.service}`,
        `Vehicle: ${data.vehicle}`,
        "—",
        data.message
      ].join("\n")
    );
    window.location.href = `mailto:${BUSINESS_EMAIL}?subject=${subject}&body=${body}`;
  };

  return (
    <div className="font-sans antialiased text-slate-900">
      {/* JSON‑LD for rich results (ok to render in body) */}
      <script type="application/ld+json" dangerouslySetInnerHTML={{ __html: JSON.stringify(ldLocalBusiness) }} />
      <script type="application/ld+json" dangerouslySetInnerHTML={{ __html: JSON.stringify(ldServices) }} />
      <script type="application/ld+json" dangerouslySetInnerHTML={{ __html: JSON.stringify(ldFAQ) }} />

      {/* NAV */}
      <header className="sticky top-0 z-50 bg-white/80 backdrop-blur border-b border-slate-100">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
          <a href="#top" className="text-lg font-extrabold tracking-tight">{BUSINESS_NAME}</a>
          <nav className="hidden md:flex items-center gap-6 text-sm">
            <a href="#services" className="hover:text-slate-700">Services</a>
            <a href="#gallery" className="hover:text-slate-700">Gallery</a>
            <a href="#reviews" className="hover:text-slate-700">Reviews</a>
            <a href="#faq" className="hover:text-slate-700">FAQ</a>
            <a href="#contact" className="hover:text-slate-700">Contact</a>
          </nav>
          <div className="flex items-center gap-3">
            <a href={`tel:${PHONE_TEL}`} className="hidden sm:inline-flex items-center rounded-full bg-black text-white px-4 py-2 text-sm font-semibold shadow hover:opacity-90">
              Call {PHONE_NUMBER}
            </a>
            <a href="#quote" className="inline-flex items-center rounded-full border border-black px-4 py-2 text-sm font-semibold hover:bg-black hover:text-white">
              Get a Quote
            </a>
          </div>
        </div>
      </header>

      {/* HERO */}
      <section id="top" className="relative overflow-hidden">
        <div className="absolute inset-0 bg-gradient-to-br from-black via-slate-900 to-slate-700" aria-hidden />
        <div className="relative max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-24 text-white">
          <div className="flex flex-col md:flex-row items-center gap-10">
            <div className="md:w-1/2">
              <div className="mb-4 flex flex-wrap gap-2">
                <Badge>Serving {CITY}, {REGION} & beyond</Badge>
                <Badge>Near Road Atlanta</Badge>
                <Badge>Enthusiast‑Owned</Badge>
              </div>
              <h1 className="text-4xl md:text-6xl font-extrabold leading-tight">
                Show‑Car Gloss. <span className="text-white/70">Zero Compromises.</span>
              </h1>
              <p className="mt-4 text-white/90 text-lg max-w-xl">
                Premium <strong>ceramic coatings</strong>, <strong>paint correction</strong>, and <strong>PPF</strong> for {CITY} and North Metro Atlanta. Trusted by enthusiasts of Tesla, Porsche, Ferrari, and more.
              </p>
              <div className="mt-6 flex flex-col sm:flex-row gap-3">
                <a href="#quote" className="inline-flex items-center rounded-full bg-white text-black px-6 py-3 font-semibold shadow hover:opacity-90">Get Instant Quote</a>
                <a href={`tel:${PHONE_TEL}`} className="inline-flex items-center rounded-full border border-white/50 px-6 py-3 font-semibold hover:bg-white/10">Call {PHONE_NUMBER}</a>
              </div>
              <div className="mt-6 flex items-center gap-3 text-white/80">
                <Stars /> <span className="text-sm">5‑Star Service • Locally Trusted</span>
              </div>
            </div>
            <div className="md:w-1/2">
              <img
                src="https://images.unsplash.com/photo-1542362567-b07e54358753?q=80&w=1600&auto=format&fit=crop"
                alt="High‑gloss sports car after ceramic coating"
                className="w-full h-[380px] object-cover rounded-2xl shadow-2xl border border-white/10"
                loading="eager"
              />
            </div>
          </div>
        </div>
      </section>

      {/* SERVICES */}
      <Section id="services" title="Signature Services" kicker="What we do best">
        <div className="grid md:grid-cols-3 gap-6">
          {[
            {
              title: "Ceramic Coatings",
              desc: "Pro‑grade protection with deep gloss and easy maintenance. Multiple packages to match your goals.",
              bullets: ["Full decon + prep", "Single/Multi‑layer options", "2–5+ year durability"],
              img: "https://images.unsplash.com/photo-1523986371872-9d3ba2e2f642?q=80&w=1200&auto=format&fit=crop"
            },
            {
              title: "Paint Correction",
              desc: "From one‑step enhancement to multi‑stage correction for swirl‑free clarity before protection.",
              bullets: ["Paint assessment", "Machine polishing", "Finish inspection"],
              img: "https://images.unsplash.com/photo-1550355291-bbee04a92027?q=80&w=1200&auto=format&fit=crop"
            },
            {
              title: "Paint Protection Film (PPF)",
              desc: "Self‑healing film for high‑impact areas. Options: partial/front clip/full body.",
              bullets: ["Rock chip defense", "Wrapped edges when possible", "Gloss or matte finishes"],
              img: "https://images.unsplash.com/photo-1516979187457-637abb4f9353?q=80&w=1200&auto=format&fit=crop"
            }
          ].map((card, idx) => (
            <div key={idx} className="group rounded-2xl overflow-hidden border border-slate-200 bg-white shadow-sm hover:shadow-lg transition">
              <img src={card.img} alt="Service" className="h-48 w-full object-cover" loading="lazy" />
              <div className="p-6">
                <h3 className="text-xl font-bold">{card.title}</h3>
                <p className="mt-2 text-slate-600">{card.desc}</p>
                <ul className="mt-3 space-y-1 text-sm text-slate-700 list-disc list-inside">
                  {card.bullets.map((b, i) => (
                    <li key={i}>{b}</li>
                  ))}
                </ul>
              </div>
            </div>
          ))}
        </div>
      </Section>

      {/* AREA & MAP */}
      <Section id="area" title="Proudly Serving North Metro Atlanta" kicker="Local expertise">
        <div className="grid md:grid-cols-2 gap-6 items-start">
          <div>
            <p className="text-slate-700">
              Based in {CITY}, we regularly serve: {SERVICE_AREAS.join(", ")} and track‑day clients near <strong>Road Atlanta</strong>.
            </p>
            <div className="mt-4 grid grid-cols-2 sm:grid-cols-3 gap-2">
              {SERVICE_AREAS.map((city) => (
                <span key={city} className="inline-flex items-center justify-center rounded-xl border px-3 py-2 text-sm">{city}</span>
              ))}
            </div>
          </div>
          <div className="rounded-2xl overflow-hidden border">
            {/* TODO: replace src with your Google Map embed for best relevance */}
            <iframe
              title="Service Area Map"
              src={`https://www.google.com/maps?q=${encodeURIComponent(CITY + ", " + REGION)}&output=embed`}
              className="w-full h-72"
              loading="lazy"
              referrerPolicy="no-referrer-when-downgrade"
            />
          </div>
        </div>
      </Section>

      {/* GALLERY */}
      <Section id="gallery" title="Recent Work" kicker="Results that speak for themselves">
        <div className="grid grid-cols-2 md:grid-cols-4 gap-3">
          {[
            "https://images.unsplash.com/photo-1542362567-b07e54358753?q=80&w=1200&auto=format&fit=crop",
            "https://images.unsplash.com/photo-1525609004556-c46c7d6cf023?q=80&w=1200&auto=format&fit=crop",
            "https://images.unsplash.com/photo-1550355291-bbee04a92027?q=80&w=1200&auto=format&fit=crop",
            "https://images.unsplash.com/photo-1517142876144-114e5e5a6781?q=80&w=1200&auto=format&fit=crop",
            "https://images.unsplash.com/photo-1517677208171-0bc6725a3e60?q=80&w=1200&auto=format&fit=crop",
            "https://images.unsplash.com/photo-1556228724-c0693fe6641f?q=80&w=1200&auto=format&fit=crop",
            "https://images.unsplash.com/photo-1552519507-88aa1e4805d9?q=80&w=1200&auto=format&fit=crop",
            "https://images.unsplash.com/photo-1543811800-9ab9c8f3a8a2?q=80&w=1200&auto=format&fit=crop"
          ].map((src, i) => (
            <img key={i} src={src} alt="Detailing result" className="aspect-square object-cover rounded-xl border" loading="lazy" />
          ))}
        </div>
      </Section>

      {/* REVIEWS */}
      <Section id="reviews" title="What Customers Say" kicker="5‑star experience">
        <div className="grid md:grid-cols-3 gap-6">
          {[
            {
              name: "J. Thompson",
              quote: "Incredible attention to detail. The paint looks better than new and washing is a breeze now.",
            },
            {
              name: "A. Patel",
              quote: "Rock‑solid communication and results. The coating transformed my Model 3.",
            },
            {
              name: "M. Harris",
              quote: "PPF edges are nearly invisible. Exactly what I wanted for track days at Road Atlanta.",
            }
          ].map((r, i) => (
            <figure key={i} className="rounded-2xl border bg-white p-6 shadow-sm">
              <Stars />
              <blockquote className="mt-3 text-slate-700">“{r.quote}”</blockquote>
              <figcaption className="mt-4 text-sm font-semibold">— {r.name}</figcaption>
            </figure>
          ))}
        </div>
        <p className="mt-6 text-sm text-slate-500">Add your real Google reviews here for stronger social proof.</p>
      </Section>

      {/* FAQ (AEO: conversational Q&A for answer engines) */}
      <Section id="faq" title="FAQs" kicker="Quick answers">
        <div className="grid md:grid-cols-2 gap-6">
          {faqs.map(({ q, a }, i) => (
            <details key={i} className="rounded-2xl border bg-white p-5">
              <summary className="cursor-pointer text-base font-semibold">{q}</summary>
              <p className="mt-2 text-slate-700 leading-relaxed">{a}</p>
            </details>
          ))}
        </div>
      </Section>

      {/* QUOTE / CONTACT */}
      <Section id="quote" title="Request a Quote" kicker="Fast, no‑pressure estimates">
        <QuoteForm onSubmit={mailtoQuote} />
        <p className="mt-4 text-sm text-slate-600">
          Prefer to call? <a className="underline" href={`tel:${PHONE_TEL}`}>{PHONE_NUMBER}</a> • Email: <a className="underline" href={`mailto:${BUSINESS_EMAIL}`}>{BUSINESS_EMAIL}</a>
        </p>
      </Section>

      {/* FOOTER */}
      <footer id="contact" className="border-t">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10 grid md:grid-cols-4 gap-8 text-sm">
          <div>
            <div className="text-lg font-extrabold">{BUSINESS_NAME}</div>
            <p className="mt-2 text-slate-600">Ceramic Coatings • Paint Correction • PPF</p>
            <p className="mt-2 text-slate-600">{CITY}, {REGION} {POSTAL}</p>
            <p className="mt-2 text-slate-600">Phone: <a className="underline" href={`tel:${PHONE_TEL}`}>{PHONE_NUMBER}</a></p>
          </div>
          <div>
            <div className="font-semibold">Company</div>
            <ul className="mt-2 space-y-2">
              <li><a href="#services" className="hover:underline">Services</a></li>
              <li><a href="#gallery" className="hover:underline">Gallery</a></li>
              <li><a href="#reviews" className="hover:underline">Reviews</a></li>
              <li><a href="#faq" className="hover:underline">FAQs</a></li>
            </ul>
          </div>
          <div>
            <div className="font-semibold">Service Area</div>
            <ul className="mt-2 grid grid-cols-2 gap-2">
              {SERVICE_AREAS.map((c) => (
                <li key={c} className="text-slate-600">{c}</li>
              ))}
            </ul>
          </div>
          <div>
            <div className="font-semibold">Hours</div>
            <ul className="mt-2 space-y-1 text-slate-600">
              <li>Mon–Fri: 9:00a – 6:00p</li>
              <li>Sat: By appointment</li>
              <li>Sun: Closed</li>
            </ul>
          </div>
        </div>
        <div className="border-t py-6 text-center text-xs text-slate-500">© {new Date().getFullYear()} {BUSINESS_NAME}. All rights reserved.</div>
      </footer>
    </div>
  );
}

function QuoteForm({ onSubmit }) {
  const [form, setForm] = useState({ name: "", email: "", phone: "", service: "Ceramic Coating", vehicle: "", message: "" });
  const [submitted, setSubmitted] = useState(false);

  const handle = (e) => setForm({ ...form, [e.target.name]: e.target.value });
  const submit = (e) => {
    e.preventDefault();
    onSubmit(form);
    setSubmitted(true);
  };

  return (
    <form onSubmit={submit} className="grid md:grid-cols-2 gap-4 bg-white p-6 rounded-2xl border shadow-sm">
      <input required name="name" onChange={handle} value={form.name} placeholder="Full name" className="border rounded-xl px-4 py-3" />
      <input required type="email" name="email" onChange={handle} value={form.email} placeholder="Email" className="border rounded-xl px-4 py-3" />
      <input required name="phone" onChange={handle} value={form.phone} placeholder="Phone" className="border rounded-xl px-4 py-3" />
      <select name="service" onChange={handle} value={form.service} className="border rounded-xl px-4 py-3">
        <option>Ceramic Coating</option>
        <option>Paint Correction</option>
        <option>PPF (Paint Protection Film)</option>
        <option>Detailing / Maintenance</option>
      </select>
      <input name="vehicle" onChange={handle} value={form.vehicle} placeholder="Vehicle (year/make/model)" className="border rounded-xl px-4 py-3 md:col-span-2" />
      <textarea name="message" onChange={handle} value={form.message} placeholder="Tell us about your goals and current paint condition…" rows={4} className="border rounded-xl px-4 py-3 md:col-span-2" />
      <div className="md:col-span-2 flex items-center justify-between">
        <button type="submit" className="inline-flex items-center rounded-full bg-black text-white px-6 py-3 font-semibold hover:opacity-90">Send Quote Request</button>
        {submitted && <span className="text-sm text-green-700">Thanks! Your email app should open—if not, email us directly.</span>}
      </div>
    </form>
  );
}
