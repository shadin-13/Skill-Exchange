import React, { useEffect, useState } from "react";

// Skill Exchange App - Single file React component (MVP)
// Uses Tailwind classes for styling. Persist data to localStorage for demo.

export default function SkillExchangeApp() {
  // user session simulation
  const [user, setUser] = useState(null);
  const [tab, setTab] = useState("home"); // home learner educator exchange
  const [profiles, setProfiles] = useState([]);
  const [posts, setPosts] = useState([]);
  const [filter, setFilter] = useState("");
  const [modal, setModal] = useState(null);

  useEffect(() => {
    const p = JSON.parse(localStorage.getItem("se_profiles") || "[]");
    const q = JSON.parse(localStorage.getItem("se_posts") || "[]");
    setProfiles(p);
    setPosts(q);
  }, []);

  useEffect(() => {
    localStorage.setItem("se_profiles", JSON.stringify(profiles));
  }, [profiles]);
  useEffect(() => {
    localStorage.setItem("se_posts", JSON.stringify(posts));
  }, [posts]);

  // quick signup for demo
  function signup(name, role, skills, wants, contact) {
    const id = Date.now().toString();
    const prof = { id, name, role, skills, wants, contact, rating: 0 };
    setProfiles((s) => [prof, ...s]);
    setUser(prof);
  }

  function createPost(ownerId, give, take, note) {
    const id = Date.now().toString();
    const post = { id, ownerId, give, take, note, createdAt: new Date().toISOString() };
    setPosts((s) => [post, ...s]);
  }

  function matchAndSchedule(post) {
    // for MVP we create a simple match object and open exchange modal
    const owner = profiles.find((p) => p.id === post.ownerId);
    setModal({ type: "match", post, owner });
  }

  function QuickDemoPopulate() {
    const demoProfiles = [
      { id: "p1", name: "Amina", role: "educator", skills: ["সেলাই"], wants: ["ইংরেজি"], contact: "+880171000000" },
      { id: "p2", name: "Rafi", role: "learner", skills: ["কম্পিউটার বেসিক"], wants: ["গ্রাফিক ডিজাইন"], contact: "+880171111111" },
      { id: "p3", name: "Sumaiya", role: "educator", skills: ["বাংলা লেখারি"], wants: ["এক্সেল"], contact: "+880171222222" },
    ];
    const demoPosts = [
      { id: "post1", ownerId: "p1", give: "সেলাই শেখাই", take: "ইংরেজি চাই", note: "সপ্তাহে ২বার", createdAt: new Date().toISOString() },
      { id: "post2", ownerId: "p2", give: "কম্পিউটার বেসিক শিখি", take: "গ্রাফিক ডিজাইন শেখাতে চাই", note: "নিয়মিত অনুশীলন দিব", createdAt: new Date().toISOString() },
    ];
    setProfiles(demoProfiles);
    setPosts(demoPosts);
    setUser(demoProfiles[1]);
  }

  return (
    <div className="min-h-screen bg-gray-50 p-6">
      <div className="max-w-4xl mx-auto bg-white shadow-md rounded-2xl overflow-hidden">
        <header className="flex items-center justify-between px-6 py-4 border-b">
          <div>
            <h1 className="text-2xl font-semibold">Skill Exchange</h1>
            <p className="text-sm text-gray-500">এক জায়গায় শিক্ষার্থী, শিক্ষক আর একবাইকে এক্সচেঞ্জ</p>
          </div>
          <div className="flex items-center gap-3">
            <nav className="flex gap-2">
              <button onClick={() => setTab("home")} className={`px-3 py-1 rounded ${tab==="home"?"bg-indigo-600 text-white":"text-gray-600"}`}>Home</button>
              <button onClick={() => setTab("learners")} className={`px-3 py-1 rounded ${tab==="learners"?"bg-indigo-600 text-white":"text-gray-600"}`}>Learners</button>
              <button onClick={() => setTab("educators")} className={`px-3 py-1 rounded ${tab==="educators"?"bg-indigo-600 text-white":"text-gray-600"}`}>Educators</button>
              <button onClick={() => setTab("exchange")} className={`px-3 py-1 rounded ${tab==="exchange"?"bg-indigo-600 text-white":"text-gray-600"}`}>Exchange 1v1</button>
            </nav>
            {user ? (
              <div className="flex items-center gap-2">
                <div className="text-sm text-gray-700">{user.name}</div>
                <button className="text-sm text-red-500" onClick={() => setUser(null)}>Logout</button>
              </div>
            ) : (
              <div className="flex gap-2">
                <button className="px-3 py-1 rounded bg-green-500 text-white text-sm" onClick={() => setModal({ type: 'signup' })}>Signup</button>
                <button className="px-3 py-1 rounded bg-gray-200 text-sm" onClick={() => QuickDemoPopulate()}>Demo</button>
              </div>
            )}
          </div>
        </header>

        <main className="p-6">
          {tab === "home" && (
            <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
              <section className="col-span-2">
                <h2 className="text-lg font-medium">Recently Posted</h2>
                <div className="mt-3 space-y-3">
                  {posts.length === 0 && <div className="text-sm text-gray-500">কোনো পোস্ট নেই। নতুন পোস্ট তৈরি করতে Signup করুন।</div>}
                  {posts.map((p) => {
                    const owner = profiles.find((x) => x.id === p.ownerId) || { name: 'Unknown' };
                    return (
                      <div key={p.id} className="border rounded p-3 flex justify-between items-start">
                        <div>
                          <div className="font-semibold">{p.give}</div>
                          <div className="text-sm text-gray-600">চাই: {p.take}</div>
                          <div className="text-xs text-gray-400 mt-1">by {owner.name}</div>
                        </div>
                        <div className="flex flex-col gap-2">
                          <button onClick={() => matchAndSchedule(p)} className="px-3 py-1 rounded bg-indigo-600 text-white text-sm">Match</button>
                          <button onClick={() => setModal({ type: 'viewPost', post: p })} className="px-3 py-1 rounded border text-sm">View</button>
                        </div>
                      </div>
                    );
                  })}
                </div>
              </section>

              <aside className="p-4 border rounded">
                <h3 className="font-medium">Create Post</h3>
                <PostForm createPost={createPost} user={user} profiles={profiles} openSignup={() => setModal({ type: 'signup' })} />
              </aside>
            </div>
          )}

          {tab === "learners" && (
            <div>
              <h2 className="text-lg font-medium">Learners</h2>
              <div className="mt-3">
                <input placeholder="সার্চ স্কিল লিখুন" value={filter} onChange={(e)=>setFilter(e.target.value)} className="w-full p-2 border rounded" />
                <div className="mt-3 space-y-3">
                  {profiles.filter(p=>p.role === 'learner' && (filter==='' || p.wants.join(' ').includes(filter))).map(p=> (
                    <ProfileCard key={p.id} p={p} onContact={()=>setModal({type:'contact', profile:p})} />
                  ))}
                </div>
              </div>
            </div>
          )}

          {tab === "educators" && (
            <div>
              <h2 className="text-lg font-medium">Educators</h2>
              <div className="mt-3">
                <input placeholder="সার্চ স্কিল লিখুন" value={filter} onChange={(e)=>setFilter(e.target.value)} className="w-full p-2 border rounded" />
                <div className="mt-3 space-y-3">
                  {profiles.filter(p=>p.role === 'educator' && (filter==='' || p.skills.join(' ').includes(filter))).map(p=> (
                    <ProfileCard key={p.id} p={p} onContact={()=>setModal({type:'contact', profile:p})} />
                  ))}
                </div>
              </div>
            </div>
          )}

          {tab === "exchange" && (
            <div>
              <h2 className="text-lg font-medium">Exchange 1v1</h2>
              <p className="text-sm text-gray-500">Match তৈরি করে ১v১ সেশন আয়োজন করুন। এখানে আপনার সব মিটিং লগ রাখা হবে।</p>
              <div className="mt-4">
                <h3 className="font-medium">Available Posts</h3>
                <div className="mt-2 space-y-2">
                  {posts.map(p=> (
                    <div key={p.id} className="border rounded p-3 flex justify-between">
                      <div>
                        <div className="font-semibold">{p.give} → {p.take}</div>
                        <div className="text-xs text-gray-400">Posted: {new Date(p.createdAt).toLocaleString()}</div>
                      </div>
                      <div className="flex gap-2">
                        <button onClick={()=>matchAndSchedule(p)} className="px-3 py-1 rounded bg-indigo-600 text-white text-sm">Match</button>
                        <button onClick={()=>setModal({type:'viewPost', post:p})} className="px-3 py-1 rounded border text-sm">View</button>
                      </div>
                    </div>
                  ))}
                </div>
              </div>
            </div>
          )}
        </main>

        <footer className="p-4 border-t text-center text-sm text-gray-500">Made for local communities. Demo only.</footer>
      </div>

      {modal && (
        <Modal onClose={()=>setModal(null)}>
          {modal.type === 'signup' && (
            <SignupForm onSubmit={(data)=>{ signup(data.name, data.role, data.skills, data.wants, data.contact); setModal(null); }} />
          )}
          {modal.type === 'viewPost' && (
            <ViewPost post={modal.post} owner={profiles.find(p=>p.id===modal.post.ownerId)} />
          )}
          {modal.type === 'contact' && (
            <ContactCard p={modal.profile} />
          )}
          {modal.type === 'match' && (
            <MatchModal post={modal.post} owner={modal.owner} onClose={()=>setModal(null)} />
          )}
        </Modal>
      )}

    </div>
  );
}

function Modal({ children, onClose }){
  return (
    <div className="fixed inset-0 bg-black bg-opacity-30 flex items-center justify-center p-4">
      <div className="bg-white rounded-lg shadow max-w-xl w-full p-4">
        <div className="flex justify-end">
          <button onClick={onClose} className="text-gray-500">Close</button>
        </div>
        <div className="mt-2">{children}</div>
      </div>
    </div>
  );
}

function SignupForm({ onSubmit }){
  const [name, setName] = useState("");
  const [role, setRole] = useState("learner");
  const [skills, setSkills] = useState("");
  const [wants, setWants] = useState("");
  const [contact, setContact] = useState("");
  return (
    <div>
      <h3 className="text-lg font-medium">Signup</h3>
      <div className="space-y-2 mt-3">
        <input value={name} onChange={(e)=>setName(e.target.value)} placeholder="আপনার নাম" className="w-full p-2 border rounded" />
        <select value={role} onChange={(e)=>setRole(e.target.value)} className="w-full p-2 border rounded">
          <option value="learner">Learner</option>
          <option value="educator">Educator</option>
          <option value="exchange">Exchange</option>
        </select>
        <input value={skills} onChange={(e)=>setSkills(e.target.value)} placeholder="আমি যা জানি (কমা দিয়ে আলাদা)" className="w-full p-2 border rounded" />
        <input value={wants} onChange={(e)=>setWants(e.target.value)} placeholder="আমি যা শিখতে চাই (কমা দিয়ে আলাদা)" className="w-full p-2 border rounded" />
        <input value={contact} onChange={(e)=>setContact(e.target.value)} placeholder="যোগাযোগ" className="w-full p-2 border rounded" />
        <div className="flex justify-end">
          <button onClick={()=> onSubmit({ name, role, skills: skills.split(',').map(s=>s.trim()).filter(Boolean), wants: wants.split(',').map(s=>s.trim()).filter(Boolean), contact })} className="px-4 py-2 bg-indigo-600 text-white rounded">Create</button>
        </div>
      </div>
    </div>
  );
}

function PostForm({ createPost, user, openSignup }){
  const [give, setGive] = useState("");
  const [take, setTake] = useState("");
  const [note, setNote] = useState("");
  return (
    <div className="space-y-2">
      <input value={give} onChange={(e)=>setGive(e.target.value)} placeholder="আমি কি শেখাতে পারি" className="w-full p-2 border rounded" />
      <input value={take} onChange={(e)=>setTake(e.target.value)} placeholder="আমি কি শিখতে চাই" className="w-full p-2 border rounded" />
      <input value={note} onChange={(e)=>setNote(e.target.value)} placeholder="কোন দিনে, কত সময়" className="w-full p-2 border rounded" />
      <div className="flex justify-end">
        {user ? (
          <button onClick={() => { createPost(user.id, give, take, note); setGive(''); setTake(''); setNote(''); }} className="px-4 py-2 bg-green-600 text-white rounded">Post</button>
        ) : (
          <button onClick={openSignup} className="px-4 py-2 bg-gray-200 text-gray-700 rounded">Signup to Post</button>
        )}
      </div>
    </div>
  );
}

function ProfileCard({ p, onContact }){
  return (
    <div className="border rounded p-3 flex justify-between items-center">
      <div>
        <div className="font-semibold">{p.name} <span className="text-xs text-gray-400">({p.role})</span></div>
        <div className="text-sm text-gray-600">Skills: {p.skills?.join(', ')}</div>
        <div className="text-sm text-gray-600">Wants: {p.wants?.join(', ')}</div>
      </div>
      <div>
        <button onClick={onContact} className="px-3 py-1 rounded border text-sm">Contact</button>
      </div>
    </div>
  );
}

function ViewPost({ post, owner }){
  return (
    <div>
      <h3 className="text-lg font-medium">Post Detail</h3>
      <div className="mt-2">
        <div><strong>Give:</strong> {post.give}</div>
        <div><strong>Take:</strong> {post.take}</div>
        <div className="text-sm text-gray-500 mt-1">By: {owner?.name || 'Unknown'}</div>
        <div className="mt-2 text-sm text-gray-600">Note: {post.note}</div>
      </div>
    </div>
  );
}

function ContactCard({ p }){
  return (
    <div>
      <h3 className="text-lg font-medium">Contact</h3>
      <div className="mt-2">
        <div className="font-semibold">{p.name}</div>
        <div className="text-sm">Contact: {p.contact}</div>
        <div className="text-sm">Skills: {p.skills?.join(', ')}</div>
      </div>
    </div>
  );
}

function MatchModal({ post, owner, onClose }){
  const [when, setWhen] = useState("");
  const [note, setNote] = useState("");
  function schedule(){
    // create a simple meeting record in localStorage for demo
    const meetings = JSON.parse(localStorage.getItem('se_meetings')||'[]');
    meetings.unshift({ id: Date.now().toString(), postId: post.id, ownerId: owner.id, when, note, link: 'https://meet.example.com/' + Date.now() });
    localStorage.setItem('se_meetings', JSON.stringify(meetings));
    alert('Match scheduled. Meeting link saved to localStorage.');
    onClose();
  }
  return (
    <div>
      <h3 className="text-lg font-medium">Match for {post.give}</h3>
      <div className="mt-2">
        <div className="text-sm text-gray-600">Owner: {owner.name}</div>
        <input value={when} onChange={(e)=>setWhen(e.target.value)} placeholder="Meeting time and date" className="w-full p-2 border rounded mt-2" />
        <input value={note} onChange={(e)=>setNote(e.target.value)} placeholder="কিছু নোট" className="w-full p-2 border rounded mt-2" />
        <div className="flex justify-end mt-3">
          <button onClick={schedule} className="px-4 py-2 bg-indigo-600 text-white rounded">Schedule</button>
        </div>
      </div>
    </div>
  );
}
