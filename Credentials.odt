"use client";

import { useState, useCallback } from "react";

// ══════════════════════════════════════════════
//  DATABASE (static mock data)
// ══════════════════════════════════════════════
type DocItem = {
  id: string;
  label: string;
  dept: string;
  icon: string;
  valid_degree: string | null;
  placeholder: string;
  detail_valid: string | null;
};

type JobItem = {
  company: string;
  designation: string;
  location: string;
  duration: string;
  current: boolean;
};

type UserData = {
  name: string;
  docs: DocItem[];
  career: {
    total_exp: string;
    jobs: JobItem[];
    stats: { companies: string; experience: string; current: string };
  };
};

const DB: Record<string, UserData> = {
  ABC2024JH001: {
    name: "Rahul Kumar Sharma",
    docs: [
      {
        id: "btech",
        label: "B.Tech Computer Science",
        dept: "Dept. of Computer Science & Engineering",
        icon: "🖥️",
        valid_degree: "BIT/BTech/CS/2023/0045",
        placeholder: "e.g. BIT/BTech/CS/2023/0045",
        detail_valid:
          "Issued: May 2023 · BIT Mesra, Ranchi<br>CGPA: 8.7 / 10",
      },
      {
        id: "mtech",
        label: "M.Tech Cyber Security",
        dept: "Dept. of Information Security",
        icon: "🔐",
        valid_degree: "NIT/MTech/IS/2025/0011",
        placeholder: "e.g. NIT/MTech/IS/2025/0011",
        detail_valid: "Issued: Jun 2025 · NIT Jamshedpur<br>Grade: A+",
      },
    ],
    career: {
      total_exp: "3 yrs 4 mos",
      jobs: [
        {
          company: "Infosys Limited",
          designation: "Senior Systems Engineer",
          location: "Pune, Maharashtra",
          duration: "Mar 2024 – Present",
          current: true,
        },
        {
          company: "Tata Consultancy Services",
          designation: "Systems Engineer",
          location: "Hyderabad, Telangana",
          duration: "Jul 2023 – Feb 2024",
          current: false,
        },
        {
          company: "Wipro Technologies",
          designation: "Project Engineer",
          location: "Bengaluru, Karnataka",
          duration: "Jan 2023 – Jun 2023",
          current: false,
        },
      ],
      stats: { companies: "3", experience: "3+ yrs", current: "Infosys" },
    },
  },
  ABC2024JH002: {
    name: "Priya Singh",
    docs: [
      {
        id: "btech",
        label: "B.Tech Computer Science",
        dept: "Dept. of Computer Science & Engineering",
        icon: "🖥️",
        valid_degree: "XLRI/BTech/CS/2024/0088",
        placeholder: "e.g. XLRI/BTech/CS/2024/0088",
        detail_valid:
          "Issued: May 2024 · XLRI Jamshedpur<br>CGPA: 9.1 / 10",
      },
      {
        id: "mtech",
        label: "M.Tech Cyber Security",
        dept: "Dept. of Information Security",
        icon: "🔐",
        valid_degree: null,
        placeholder: "Enter degree number",
        detail_valid: null,
      },
    ],
    career: {
      total_exp: "1 yr 2 mos",
      jobs: [
        {
          company: "HCL Technologies",
          designation: "Graduate Engineer Trainee",
          location: "Noida, Uttar Pradesh",
          duration: "Aug 2024 – Present",
          current: true,
        },
        {
          company: "Capgemini India",
          designation: "Security Analyst Intern",
          location: "Mumbai, Maharashtra",
          duration: "Jan 2024 – Jun 2024",
          current: false,
        },
      ],
      stats: {
        companies: "2",
        experience: "1+ yr",
        current: "HCL Technologies",
      },
    },
  },
};

function initials(n: string): string {
  return n
    .split(" ")
    .slice(0, 2)
    .map((w) => w[0])
    .join("")
    .toUpperCase();
}

export default function EduTrustVerificationPage() {
  const [abcInput, setAbcInput] = useState("");
  const [abcError, setAbcError] = useState(false);
  const [user, setUser] = useState<{ id: string } & UserData | null>(null);
  const [selectedDoc, setSelectedDoc] = useState<DocItem | null>(null);
  const [degreeNo, setDegreeNo] = useState("");
  const [degreeError, setDegreeError] = useState(false);
  const [activeTab, setActiveTab] = useState<"academic" | "career">("academic");
  const [panel, setPanel] = useState<1 | 2 | 3>(1);
  const [result, setResult] = useState<{
    valid: boolean;
    status: string;
    title: string;
    details: string;
  } | null>(null);

  const step = panel;
  const progressWidth = step === 1 ? "33%" : step === 2 ? "66%" : "100%";

  const lookupID = useCallback(() => {
    const raw = abcInput.trim().toUpperCase();
    const data = DB[raw];

    if (!data) {
      setAbcError(true);
      setAbcInput("");
      setTimeout(() => {
        setAbcError(false);
      }, 2500);
      return;
    }

    setUser({ id: raw, ...data });
    setSelectedDoc(null);
    setDegreeNo("");
    setResult(null);
    setActiveTab("academic");
    setPanel(2);
  }, [abcInput]);

  const verifyDoc = useCallback(() => {
    if (!selectedDoc) {
      return;
    }

    const deg = degreeNo.trim();
    if (!deg) {
      setDegreeError(true);
      setTimeout(() => setDegreeError(false), 1800);
      return;
    }

    const exists = selectedDoc.valid_degree !== null;
    const ok =
      exists &&
      deg.toUpperCase() === selectedDoc.valid_degree!.toUpperCase();

    if (ok) {
      setResult({
        valid: true,
        status: "Document Verified",
        title: `${selectedDoc.label} — ${user!.name}`,
        details: `<strong>Status:</strong> Authentic record found in EduTrust registry<br>${selectedDoc.detail_valid}<br>Degree No: <strong>${deg}</strong>`,
      });
    } else if (!exists) {
      setResult({
        valid: false,
        status: "Verification Failed",
        title: `${selectedDoc.label} — ${user!.name}`,
        details:
          "<strong>Status:</strong> This document does not exist in the registry.<br>No record of this degree was found for this student.",
      });
    } else {
      setResult({
        valid: false,
        status: "Verification Failed",
        title: `${selectedDoc.label} — ${user!.name}`,
        details:
          "<strong>Status:</strong> Degree number did not match our records.<br>Please check the certificate number and try again.",
      });
    }

    setPanel(3);
  }, [selectedDoc, degreeNo, user]);

  const reset = useCallback(() => {
    setUser(null);
    setSelectedDoc(null);
    setAbcInput("");
    setDegreeNo("");
    setResult(null);
    setPanel(1);
  }, []);

  const selectDoc = useCallback((doc: DocItem) => {
    setSelectedDoc(doc);
    setDegreeNo("");
  }, []);

  const handleAbcKeyDown = (e: React.KeyboardEvent) => {
    if (e.key === "Enter") lookupID();
  };

  const handleDegreeKeyDown = (e: React.KeyboardEvent) => {
    if (e.key === "Enter") verifyDoc();
  };

  return (
    <>
      <div className="topbar">
        <div className="logo">
          <div className="logo-mark">
            <svg viewBox="0 0 18 18" fill="none">
              <path
                d="M9 1L11.5 6.5H17L12.5 10L14.5 16L9 12.5L3.5 16L5.5 10L1 6.5H6.5L9 1Z"
                fill="white"
                fillOpacity={0.9}
              />
            </svg>
          </div>
          <div className="logo-name">
            Edu<span>Trust</span>
          </div>
        </div>
        <div className="gov-tag">
          <div className="gov-dot" />
          Jharkhand · Employer Portal
        </div>
      </div>

      <div className="card">
        <div className="card-top">
          <div>
            <div className="card-title">Academic Credential Verification</div>
            <div className="card-sub">
              Verify a student&apos;s degree or career history using their ABC Student ID
            </div>
          </div>
          <div className="step-badge">Step {step} of 3</div>
        </div>

        <div className="progress-track">
          <div
            className="progress-fill"
            style={{ width: progressWidth }}
          />
        </div>

        <div className="steps">
          <div className={`step ${step === 1 ? "active" : step > 1 ? "done" : ""}`}>
            <div className="step-inner">
              <div className="step-num">1</div>
              <div className="step-label">Identity</div>
            </div>
          </div>
          <div className="step-connector" />
          <div className={`step ${step === 2 ? "active" : step > 2 ? "done" : ""}`}>
            <div className="step-inner">
              <div className="step-num">2</div>
              <div className="step-label">Verification</div>
            </div>
          </div>
          <div className="step-connector" />
          <div className={`step ${step === 3 ? "active" : ""}`}>
            <div className="step-inner">
              <div className="step-num">3</div>
              <div className="step-label">Result</div>
            </div>
          </div>
        </div>

        <div className="card-body">
          {/* Panel 1 */}
          <div id="panel1" className={panel === 1 ? "" : "hidden"}>
            <div className="field">
              <label htmlFor="abcInput">ABC Student ID</label>
              <input
                id="abcInput"
                type="text"
                value={abcInput}
                onChange={(e) => setAbcInput(e.target.value)}
                onKeyDown={handleAbcKeyDown}
                placeholder={abcError ? "ABC ID not found — try again" : "e.g. ABC2024JH001"}
                className={abcError ? "error" : ""}
                autoComplete="off"
              />
              <div className="helper">
                Enter the ABC ID provided by the employee or candidate.
              </div>
            </div>
            <button className="btn btn-primary" type="button" onClick={lookupID}>
              Look Up Student &nbsp;→
            </button>
          </div>

          {/* Panel 2 */}
          <div id="panel2" className={panel === 2 ? "" : "hidden"}>
            {user && (
              <>
                <div className="identity-pill">
                  <div className="id-avatar">{initials(user.name)}</div>
                  <div>
                    <div className="id-name">{user.name}</div>
                    <div className="id-code">ABC ID: {user.id}</div>
                  </div>
                  <div className="id-verified">Verified</div>
                </div>

                <div className="tabs">
                  <button
                    type="button"
                    className={`tab ${activeTab === "academic" ? "active" : ""}`}
                    onClick={() => setActiveTab("academic")}
                  >
                    🎓 &nbsp;Academic
                  </button>
                  <button
                    type="button"
                    className={`tab ${activeTab === "career" ? "active" : ""}`}
                    onClick={() => setActiveTab("career")}
                  >
                    💼 &nbsp;Professional History
                  </button>
                </div>

                {/* Academic tab */}
                <div
                  id="tabContentAcademic"
                  className={activeTab === "academic" ? "" : "hidden"}
                >
                  <div className="section-label">Select document to verify</div>
                  <div className="doc-options">
                    {user.docs.map((doc) => (
                      <div
                        key={doc.id}
                        role="button"
                        tabIndex={0}
                        className={`doc-option ${selectedDoc?.id === doc.id ? "selected" : ""}`}
                        onClick={() => selectDoc(doc)}
                        onKeyDown={(e) =>
                          e.key === "Enter" && selectDoc(doc)
                        }
                      >
                        <div className="doc-icon">{doc.icon}</div>
                        <div>
                          <div className="doc-title">{doc.label}</div>
                          <div className="doc-dept">{doc.dept}</div>
                        </div>
                        <div className="doc-radio" />
                      </div>
                    ))}
                  </div>

                  {selectedDoc && (
                    <div
                      className="field"
                      style={{ marginTop: "1.2rem" }}
                    >
                      <label htmlFor="inpDegreeNo">Degree / Certificate Number</label>
                      <input
                        id="inpDegreeNo"
                        type="text"
                        value={degreeNo}
                        onChange={(e) => setDegreeNo(e.target.value)}
                        onKeyDown={handleDegreeKeyDown}
                        placeholder={selectedDoc.placeholder}
                        className={degreeError ? "error" : ""}
                        autoComplete="off"
                      />
                      <div className="helper">
                        Enter the number exactly as it appears on the certificate.
                      </div>
                    </div>
                  )}

                  <button
                    type="button"
                    className="btn btn-primary"
                    onClick={verifyDoc}
                  >
                    Verify Document &nbsp;🔍
                  </button>
                  <button type="button" className="btn btn-ghost" onClick={reset}>
                    ← Start over
                  </button>
                </div>

                {/* Career tab */}
                <div
                  id="tabContentCareer"
                  className={activeTab === "career" ? "" : "hidden"}
                >
                  <div className="career-header">
                    <div className="section-label" style={{ marginBottom: 0 }}>
                      Employment Records
                    </div>
                    <div className="career-total">
                      Total: <span>{user.career.total_exp}</span>
                    </div>
                  </div>

                  <div className="timeline">
                    {user.career.jobs.map((job) => (
                      <div key={job.company} className="timeline-item">
                        <div className="tl-left">
                          <div
                            className={`tl-dot ${
                              job.current ? "current" : "past"
                            }`}
                          >
                            {job.current ? "●" : "○"}
                          </div>
                        </div>
                        <div className="tl-right">
                          <div
                            className={`job-card ${
                              job.current ? "current-job" : ""
                            }`}
                          >
                            <div className="job-row-top">
                              <div className="job-company">{job.company}</div>
                              <div
                                className={`job-status-badge ${
                                  job.current ? "badge-current" : "badge-verified"
                                }`}
                              >
                                {job.current ? "Current" : "Verified"}
                              </div>
                            </div>
                            <div className="job-designation">
                              {job.designation}
                            </div>
                            <div className="job-chips">
                              <div className="chip">
                                <span className="chip-icon">📅</span>
                                {job.duration}
                              </div>
                              <div className="chip">
                                <span className="chip-icon">📍</span>
                                {job.location}
                              </div>
                            </div>
                          </div>
                        </div>
                      </div>
                    ))}
                  </div>

                  <div className="career-summary">
                    <div className="sum-stat">
                      <div className="sum-val">{user.career.stats.companies}</div>
                      <div className="sum-lbl">Companies</div>
                    </div>
                    <div className="sum-stat">
                      <div className="sum-val">{user.career.stats.experience}</div>
                      <div className="sum-lbl">Experience</div>
                    </div>
                    <div className="sum-stat" style={{ flex: 2 }}>
                      <div
                        className="sum-val"
                        style={{
                          fontSize: "0.82rem",
                          color: "var(--green)",
                        }}
                      >
                        {user.career.stats.current}
                      </div>
                      <div className="sum-lbl">Currently at</div>
                    </div>
                  </div>

                  <button
                    type="button"
                    className="btn btn-ghost"
                    style={{ marginTop: "1rem" }}
                    onClick={reset}
                  >
                    ← Start over
                  </button>
                </div>
              </>
            )}
          </div>

          {/* Panel 3 */}
          <div id="panel3" className={panel === 3 ? "" : "hidden"}>
            {result && (
              <div className={`result-box ${result.valid ? "valid" : "invalid"}`}>
                <div className="result-icon-wrap">
                  {result.valid ? "✅" : "❌"}
                </div>
                <div className="result-status">{result.status}</div>
                <div className="result-title">{result.title}</div>
                <div
                  className="result-details"
                  dangerouslySetInnerHTML={{ __html: result.details }}
                />
              </div>
            )}
            <button
              type="button"
              className="btn btn-ghost"
              style={{ marginTop: "1rem" }}
              onClick={reset}
            >
              ← Verify another
            </button>
          </div>
        </div>
      </div>

      <footer className="edu-footer">
        EduTrust · Jharkhand Higher Education · Demo Build · No real data stored
      </footer>
    </>
  );
}
