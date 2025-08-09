# eklipseeducation
import React, { useMemo, useState, useEffect } from "react";
import { motion, AnimatePresence } from "framer-motion";
import { Heart, Smile, Search, Sparkles, X, ChevronLeft, ChevronRight } from "lucide-react";

// ---- Data (from your slides) ----
const STORIES = [
  {
    name: "Priyansh",
    tag: "Joyful energy",
    images: [
      "https://placehold.co/800x600?text=Priyansh+1",
      "https://placehold.co/800x600?text=Priyansh+2",
    ],
    text:
      "I met Priyansh on my first day at Manav Seva. His energy immediately caught my attention as he was always doing everything with the biggest smile on his face. I spent the day playing games with the children. My seat was right next to Priyansh and we would regularly talk about different things in our day to day life. We formed a friendship which became stronger as my time at the center went on. He asked for me to come over to his home, which I did later during my stay. The day that I came over to his house, he energetically showed me around. I keenly observed the neat and clean way in which his home was kept. It was a tiny two story condo with one bed right above the kitchen. Although not having much, his mom gave him a few rupees in order to treat me and Sejal Di, the volunteer who came with me, with a drink and candy. This felt like true 'prasad', known as a blessing or gift for God. It really stuck with me that although his family had barely enough room for themselves, they found it in their hearts to be exceedingly hospitable and generous.",
  },
  {
    name: "Jaysheel",
    tag: "Innocent joy",
    images: [
      "https://placehold.co/800x600?text=Jaysheel+1",
      "https://placehold.co/800x600?text=Jaysheel+2",
    ],
    text:
      "Jaysheel came to me one morning during the second week of my stay. He invited me to join in on a fun game, the Indian alternative of 'four corners'. From that day onwards, I had many conversations with him and learnt of his life and adventures. He is a dedicated and fun young boy who always looks at the best in every situation. His smile is so innocent and can truly light up anyone's day.",
  },
  {
    name: "Three Girls",
    tag: "Resilient smiles",
    images: [
      "https://placehold.co/800x600?text=Three+Girls+1",
      "https://placehold.co/800x600?text=Three+Girls+2",
    ],
    text:
      "These three girls asked me to take a picture of them on my last day at the center. Despite their hardships they smile. Their ability to stay happy no matter what is evident in the picture, showcasing the true value of maintaining happiness. This is something that I learned from them, and their energy truly made me feel at home.",
  },
  {
    name: "Prachi",
    tag: "Boundless energy",
    images: [
      "https://placehold.co/800x600?text=Prachi+1",
      "https://placehold.co/800x600?text=Prachi+2",
    ],
    text:
      "The one word which describes Prachi is energetic. I could always hear her shouting my name to have me join her in whatever game she was playing. In fact, this was a characteristic shared by many of the younger girls. They would rush to my side and fight for whoever got a chance to have all of us play their game.",
  },
  {
    name: "Kinjal",
    tag: "Eager learner",
    images: [
      "https://placehold.co/800x600?text=Kinjal+1",
      "https://placehold.co/800x600?text=Kinjal+2",
    ],
    text:
      "Kinjal came across as very eager to learn. Whenever I was appointed the task of teaching math to the third and fourth graders, I could always count on Kinjal participating in the activities. Her engagement and dedication to learning inspired me. Despite never having been granted abundant opportunities, she embraced everything offered to her with utmost respect and devotion.",
  },
  {
    name: "Riya",
    tag: "Thoughtful helper",
    images: [
      "https://placehold.co/800x600?text=Riya+1",
      "https://placehold.co/800x600?text=Riya+2",
    ],
    text:
      "Riya's smile always stood out to me. It had this simple yet powerful ability to make me smile in return. She was always happy and thoughtful, thinking about those around her and what she can do to help them out.",
  },
  {
    name: "Vihaan",
    tag: "Big heart, bigger laugh",
    images: [
      "https://placehold.co/800x600?text=Vihaan+1",
      "https://placehold.co/800x600?text=Vihaan+2",
    ],
    text:
      "Vihaan had this certain smile that had the power to transcend everything around him. His energy was so vast and pervasive that you can easily distinguish his voice from miles away. I met Vihaan on my first day at Manav Seva. He caught my eye as he first smiled at me and then, when I smiled back, he got shy and hid his face. From then on, I couldn't help but smile every time I looked at him. He would shy away, and then call his friends over to come look alongside him. Finally we spoke and from that day we were inseparable. We would race together, play cricket together, and do math together. I saw that he tried really hard to solve math problems, although it didn't come easy to him. At times, he would feel tempted to take the easy way out and cheat, but this was only because he felt pushed in that direction. I tried to encourage him, reassuring him that it is okay to make mistakes as long as you learn from them and don't repeat them. In teaching him this lesson I realized that I was learning something myself. Vihaan holds a special place in my heart and I am eternally grateful for that.",
  },
];

// ---- Small helpers ----
const useDarkMode = () => {
  const [dark, setDark] = useState(true);
  useEffect(() => {
    if (dark) document.documentElement.classList.add("dark");
    else document.documentElement.classList.remove("dark");
  }, [dark]);
  return { dark, setDark };
};

const Badge = ({ children }) => (
  <span className="inline-flex items-center gap-1 rounded-full bg-blue-100/60 dark:bg-blue-500/20 px-3 py-1 text-xs font-medium text-blue-700 dark:text-blue-200 ring-1 ring-inset ring-blue-300/50">
    <Sparkles className="h-3 w-3" /> {children}
  </span>
);

const GlassCard = ({ children, onClick }) => (
  <motion.div
    onClick={onClick}
    whileHover={{ y: -4, scale: 1.01 }}
    transition={{ type: "spring", stiffness: 300, damping: 20 }}
    className="group cursor-pointer rounded-2xl p-5 backdrop-blur-xl bg-white/60 dark:bg-slate-900/50 shadow-[0_10px_30px_rgba(0,0,0,0.1)] border border-white/40 dark:border-slate-700/50"
  >
    {children}
  </motion.div>
);

const Modal = ({ open, onClose, story }) => (
  <AnimatePresence>
    {open && story && (
      <motion.div
        initial={{ opacity: 0 }}
        animate={{ opacity: 1 }}
        exit={{ opacity: 0 }}
        className="fixed inset-0 z-50 flex items-center justify-center bg-slate-900/60 p-4"
        aria-modal
        role="dialog"
      >
        <motion.div
          initial={{ y: 20, opacity: 0 }}
          animate={{ y: 0, opacity: 1 }}
          exit={{ y: 20, opacity: 0 }}
          className="max-h-[85vh] w-full max-w-3xl overflow-hidden rounded-3xl bg-white dark:bg-slate-900 shadow-2xl border border-slate-200 dark:border-slate-700"
        >
          <div className="flex items-center justify-between border-b border-slate-200 dark:border-slate-700 p-4">
            <div className="flex items-center gap-2 text-slate-800 dark:text-slate-100">
              <Smile className="h-5 w-5" />
              <h3 className="text-lg font-semibold">{story.name}</h3>
            </div>
            <button
              onClick={onClose}
              className="rounded-full p-2 hover:bg-slate-100 dark:hover:bg-slate-800"
              aria-label="Close"
            >
              <X className="h-5 w-5" />
            </button>
          </div>

          <div className="grid gap-0 md:grid-cols-2">
            <Carousel images={story.images} />
            <div className="p-6">
              <Badge>{story.tag}</Badge>
              <p className="mt-4 text-slate-700 dark:text-slate-200 leading-7">
                {story.text}
              </p>
            </div>
          </div>
        </motion.div>
      </motion.div>
    )}
  </AnimatePresence>
);

const Carousel = ({ images = [] }) => {
  const [idx, setIdx] = useState(0);
  const prev = () => setIdx((i) => (i - 1 + images.length) % images.length);
  const next = () => setIdx((i) => (i + 1) % images.length);

  return (
    <div className="relative aspect-[4/3] w-full overflow-hidden bg-gradient-to-br from-blue-100 to-blue-200 dark:from-slate-800 dark:to-slate-900">
      <AnimatePresence mode="wait">
        <motion.img
          key={images[idx]}
          src={images[idx]}
          alt=""
          initial={{ opacity: 0, scale: 1.02 }}
          animate={{ opacity: 1, scale: 1 }}
          exit={{ opacity: 0, scale: 0.98 }}
          transition={{ duration: 0.35 }}
          className="absolute inset-0 h-full w-full object-cover"
        />
      </AnimatePresence>
      <button
        onClick={prev}
        className="absolute left-3 top-1/2 -translate-y-1/2 rounded-full bg-white/80 dark:bg-slate-900/80 p-2 backdrop-blur hover:bg-white dark:hover:bg-slate-900"
        aria-label="Previous"
      >
        <ChevronLeft className="h-5 w-5" />
      </button>
      <button
        onClick={next}
        className="absolute right-3 top-1/2 -translate-y-1/2 rounded-full bg-white/80 dark:bg-slate-900/80 p-2 backdrop-blur hover:bg-white dark:hover:bg-slate-900"
        aria-label="Next"
      >
        <ChevronRight className="h-5 w-5" />
      </button>
    </div>
  );
};

export default function ManavSevaStories() {
  const { dark, setDark } = useDarkMode();
  const [query, setQuery] = useState("");
  const [active, setActive] = useState(null);

  const filtered = useMemo(() => {
    if (!query.trim()) return STORIES;
    return STORIES.filter((s) =>
      (s.name + " " + s.tag + " " + s.text)
        .toLowerCase()
        .includes(query.toLowerCase())
    );
  }, [query]);

  return (
    <div className="min-h-screen bg-gradient-to-b from-blue-50 via-white to-blue-50 dark:from-slate-950 dark:via-slate-950 dark:to-slate-900 text-slate-900 dark:text-slate-50">
      {/* Top Bar */}
      <header className="sticky top-0 z-40 backdrop-blur supports-[backdrop-filter]:bg-white/60 dark:supports-[backdrop-filter]:bg-slate-900/50 border-b border-white/40 dark:border-slate-800">
        <div className="mx-auto max-w-7xl px-4 py-3 flex items-center gap-3">
          <div className="flex items-center gap-2">
            <div className="h-8 w-8 rounded-xl bg-gradient-to-br from-blue-500 to-indigo-600 grid place-items-center shadow-lg">
              <Heart className="h-4 w-4 text-white" />
            </div>
            <span className="text-sm font-semibold tracking-tight">Manav Seva Stories</span>
          </div>
          <div className="ml-auto flex items-center gap-2">
            <div className="relative hidden sm:block">
              <Search className="absolute left-3 top-1/2 -translate-y-1/2 h-4 w-4 text-slate-400" />
              <input
                value={query}
                onChange={(e) => setQuery(e.target.value)}
                placeholder="Search names, traits, moments..."
                className="w-72 rounded-xl border border-slate-200 dark:border-slate-700 bg-white/70 dark:bg-slate-900/60 pl-9 pr-3 py-2 text-sm outline-none focus:ring-2 focus:ring-blue-400"
              />
            </div>
            <button
              onClick={() => setDark(!dark)}
              className="rounded-xl border border-slate-200 dark:border-slate-700 bg-white/70 dark:bg-slate-900/60 px-3 py-2 text-xs font-medium hover:ring-2 hover:ring-blue-400"
            >
              {dark ? "Light" : "Dark"} mode
            </button>
          </div>
        </div>
      </header>

      {/* Hero */}
      <section className="relative overflow-hidden">
        <div className="absolute inset-0 -z-10 bg-[radial-gradient(60%_60%_at_50%_10%,rgba(37,99,235,0.25),transparent_60%)]" />
        <div className="mx-auto max-w-7xl px-4 py-16 sm:py-24 grid gap-10 md:grid-cols-2 items-center">
          <div>
            <Badge>Community • Joy • Learning</Badge>
            <h1 className="mt-4 text-4xl sm:text-5xl font-extrabold tracking-tight bg-clip-text text-transparent bg-gradient-to-b from-blue-700 to-indigo-800 dark:from-blue-300 dark:to-indigo-200">
              Moments that feel like home
            </h1>
            <p className="mt-4 text-lg leading-8 text-slate-700 dark:text-slate-300">
              A modern, web-style storytelling page that captures the smiles, lessons,
              and little miracles from days at Manav Seva.
            </p>
            <div className="mt-6 flex flex-wrap items-center gap-3">
              <a href="#stories" className="rounded-xl bg-gradient-to-r from-blue-600 to-indigo-600 px-5 py-3 text-sm font-semibold text-white shadow hover:opacity-95">Explore stories</a>
              <a href="#about" className="rounded-xl border border-slate-300/80 dark:border-slate-700 px-5 py-3 text-sm font-semibold hover:bg-white/60 dark:hover:bg-slate-900/40">About the project</a>
            </div>
            <div className="mt-8 flex items-center gap-6 text-sm text-slate-600 dark:text-slate-400">
              <div className="flex items-center gap-2"><Smile className="h-4 w-4" /> 7 heartfelt portraits</div>
              <div className="flex items-center gap-2"><Sparkles className="h-4 w-4" /> Built with love</div>
            </div>
          </div>
          <motion.div
            initial={{ opacity: 0, y: 20 }}
            whileInView={{ opacity: 1, y: 0 }}
            viewport={{ once: true }}
            transition={{ duration: 0.6 }}
            className="relative"
          >
            <div className="aspect-[4/3] w-full rounded-3xl bg-gradient-to-br from-blue-200 via-blue-100 to-indigo-200 dark:from-slate-800 dark:via-slate-900 dark:to-slate-800 p-2 shadow-xl">
              <div className="h-full w-full rounded-2xl bg-white/40 dark:bg-slate-900/40 backdrop-blur-xl border border-white/50 dark:border-slate-700" />
            </div>
            <p className="mt-3 text-center text-xs text-slate-500 dark:text-slate-400">Drop photos into cards later — placeholders shown.</p>
          </motion.div>
        </div>
      </section>

      {/* Quick stats */}
      <section id="about" className="mx-auto max-w-7xl px-4 py-10">
        <div className="grid gap-4 sm:grid-cols-3">
          {[
            { k: "+30", v: "days with the community" },
            { k: "100%", v: "smiles returned" },
            { k: "∞", v: "gratitude learned" },
          ].map((s) => (
            <div key={s.v} className="rounded-2xl bg-white/70 dark:bg-slate-900/60 backdrop-blur border border-slate-200/60 dark:border-slate-700 p-6 text-center shadow-sm">
              <div className="text-3xl font-black tracking-tight bg-clip-text text-transparent bg-gradient-to-b from-blue-700 to-indigo-700 dark:from-blue-300 dark:to-indigo-200">
                {s.k}
              </div>
              <div className="mt-1 text-sm text-slate-600 dark:text-slate-300">{s.v}</div>
            </div>
          ))}
        </div>
      </section>

      {/* Stories grid */}
      <section id="stories" className="mx-auto max-w-7xl px-4 py-16">
        <div className="mb-8 flex flex-col sm:flex-row sm:items-end sm:justify-between gap-4">
          <h2 className="text-2xl sm:text-3xl font-bold tracking-tight">Featured Stories</h2>
          <div className="relative sm:hidden">
            <Search className="absolute left-3 top-1/2 -translate-y-1/2 h-4 w-4 text-slate-400" />
            <input
              value={query}
              onChange={(e) => setQuery(e.target.value)}
              placeholder="Search stories..."
              className="w-full rounded-xl border border-slate-200 dark:border-slate-700 bg-white/70 dark:bg-slate-900/60 pl-9 pr-3 py-2 text-sm outline-none focus:ring-2 focus:ring-blue-400"
            />
          </div>
        </div>

        <div className="grid gap-6 sm:grid-cols-2 lg:grid-cols-3">
          {filtered.map((s, i) => (
            <GlassCard key={s.name} onClick={() => setActive(s)}>
              <div className="aspect-[4/3] w-full overflow-hidden rounded-xl bg-gradient-to-br from-blue-200 to-indigo-200 dark:from-slate-800 dark:to-slate-900">
                {/* Placeholder image area - replace with actual photos later */}
              </div>
              <div className="mt-4 flex items-start justify-between gap-3">
                <div>
                  <h3 className="text-lg font-semibold leading-tight">{s.name}</h3>
                  <p className="mt-1 text-sm text-slate-600 dark:text-slate-300">{s.tag}</p>
                </div>
                <Badge>Read</Badge>
              </div>
              <p className="mt-3 line-clamp-3 text-sm text-slate-700 dark:text-slate-200">
                {s.text}
              </p>
            </GlassCard>
          ))}
        </div>

        {filtered.length === 0 && (
          <p className="mt-8 text-center text-slate-500">No matches. Try another search.</p>
        )}
      </section>

      {/* Callout strip */}
      <section className="mx-auto max-w-7xl px-4 pb-16">
        <div className="rounded-3xl bg-gradient-to-r from-blue-600 to-indigo-600 text-white p-8 sm:p-12 shadow-xl">
          <div className="grid gap-6 md:grid-cols-2 items-center">
            <div>
              <h3 className="text-2xl font-bold">Small acts. Big impact.</h3>
              <p className="mt-2 text-sm/relaxed text-white/90">
                These memories are living proof that kindness multiplies. Use this page as
                a digital keepsake or convert it back into slides — it's designed for both.
              </p>
            </div>
            <div className="flex flex-wrap gap-3">
              <a href="#stories" className="rounded-xl bg-white text-blue-700 font-semibold px-4 py-2 shadow hover:bg-white/90">Browse all</a>
              <button className="rounded-xl border border-white/60 bg-white/10 px-4 py-2 font-semibold hover:bg-white/20">
                Add photos later
              </button>
            </div>
          </div>
        </div>
      </section>

      {/* Footer */}
      <footer className="border-t border-slate-200/60 dark:border-slate-800">
        <div className="mx-auto max-w-7xl px-4 py-10 grid gap-6 sm:grid-cols-3">
          <div>
            <div className="flex items-center gap-2">
              <div className="h-8 w-8 rounded-xl bg-gradient-to-br from-blue-500 to-indigo-600 grid place-items-center shadow-lg">
                <Heart className="h-4 w-4 text-white" />
              </div>
              <span className="text-sm font-semibold tracking-tight">Manav Seva Stories</span>
            </div>
            <p className="mt-3 text-sm text-slate-600 dark:text-slate-300">
              A modern, blue-themed, website-style presentation.
            </p>
          </div>
          <div>
            <h4 className="text-sm font-semibold">Quick Links</h4>
            <ul className="mt-2 space-y-2 text-sm text-slate-600 dark:text-slate-300">
              <li><a href="#about" className="hover:underline">About</a></li>
              <li><a href="#stories" className="hover:underline">Stories</a></li>
              <li><a href="#" className="hover:underline">Add media</a></li>
            </ul>
          </div>
          <div>
            <h4 className="text-sm font-semibold">Built With</h4>
            <ul className="mt-2 space-y-2 text-sm text-slate-600 dark:text-slate-300">
              <li>React + Tailwind + Framer Motion</li>
              <li>Glassmorphism + gradients + dark mode</li>
              <li>Accessible modals and keyboard-friendly UI</li>
            </ul>
          </div>
        </div>
        <div className="py-6 text-center text-xs text-slate-500">
          © {new Date().getFullYear()} Manav Seva Stories. All rights reserved.
        </div>
      </footer>

      <Modal open={!!active} onClose={() => setActive(null)} story={active} />

      {/* Floating action */}
      <a
        href="#stories"
        className="fixed bottom-6 right-6 rounded-2xl bg-gradient-to-br from-blue-600 to-indigo-600 text-white px-4 py-3 shadow-2xl hover:opacity-95"
      >
        Explore Stories
      </a>
    </div>
  );
}
