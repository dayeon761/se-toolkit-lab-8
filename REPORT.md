# Lab 8 Report

## Task 1A — Bare agent

**Question:** What is the agentic loop?
**Answer:**
The agentic loop is the fundamental cycle that autonomous AI agents follow to accomplish tasks. It typically consists of these core stages:

1. **Perceive** — Gather information from the environment (user input, tool outputs, file contents, API responses, etc.)
2. **Reason/Plan** — Analyze the information, decide what to do next, and formulate a plan or action
3. **Act** — Execute actions using available tools (file operations, API calls, shell commands, web searches, etc.)
4. **Observe** — Receive feedback from the action (success, failure, output data)
5. **Repeat** — Loop back to step 1 with the new information, continuing until the task is complete

**Question:** What labs are available in our LMS?
**Answer:**
(Агент не знал — галлюцинировал или сказал "I don't know")

## Task 1B — Agent with LMS tools

**Question:** What labs are available?
**Answer:**
Here are the available labs in the LMS:

| ID | Title |
|----|-------|
| 1 | Lab 01 – Products, Architecture & Roles |
| 2 | Lab 02 — Run, Fix, and Deploy a Backend Service |
| 3 | Lab 03 — Backend API: Explore, Debug, Implement, Deploy |
| 4 | Lab 04 — Testing, Front-end, and AI Agents |
| 5 | Lab 05 — Data Pipeline and Analytics Dashboard |
| 6 | Lab 06 — Build Your Own Agent |
| 7 | Lab 07 — Build a Client with an AI Coding Agent |
| 8 | lab-08 |

**Question:** Describe the architecture of the LMS system
**Answer:**
  ↳ I'll explore the workspace to understand the LMS system architecture.

🐈 nanobot
Based on my exploration, here's the LMS (Learning Management System) architecture:                    

------------------------------------------------------------------------------------------------------

🏗️ LMS System Architecture                                                                            

High-Level Overview                                                                                   

                                                                                                      
 ┌─────────────────────────────────────────────────────────────────────────┐                          
 │                           CLIENTS                                        │                         
 │  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌─────────────┐ │                           
 │  │   React      │  │   Flutter    │  │   Nanobot    │  │   Swagger   │ │                           
 │  │   Dashboard  │  │   Web Chat   │  │   Agent      │  │   UI        │ │                           
 │  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘  └──────┬──────┘ │                           
 └─────────┼─────────────────┼─────────────────┼─────────────────┼────────┘                           
           │                 │                 │                 │                                    
           └─────────────────┴────────┬────────┴─────────────────┘                                    
                                      │                                                               
                           ┌──────────▼──────────┐                                                    
                           │      Caddy          │                                                    
                           │  (Reverse Proxy)    │                                                    
                           └──────────┬──────────┘                                                    
                                      │                                                               
           ┌──────────────────────────┼──────────────────────────┐                                    
           │                          │                          │                                    
 ┌─────────▼─────────┐    ┌──────────▼──────────┐    ┌──────────▼──────────┐                          
 │  Backend (FastAPI)│    │  Qwen Code API      │    │  Observability      │                          
 │                   │    │  (LLM Gateway)      │    │  Stack              │                          
 │  - /items         │    │                     │    │                     │                          
 │  - /interactions  │    │                     │    │  ┌───────────────┐  │                          
 │  - /learners      │    │                     │    │  │ OTel Collector│  │                          
 │  - /pipeline      │    │                     │    │  └───────┬───────┘  │                          
 │  - /analytics     │    │                     │    │          │          │                          
 └─────────┬─────────┘    └─────────────────────┘    │  ┌───────▼───────┐  │                          
           │                                         │  │ VictoriaLogs  │  │                          
           │                                         │  │ VictoriaTraces│  │                          
 ┌─────────▼─────────┐                               │  └───────────────┘  │                          
 │    PostgreSQL     │                               └─────────────────────┘                          
 │    (Database)     │                                                                                
 │                   │                                                                                
 │  - learners       │                                                                                
 │  - interactions   │                                                                                
 │  - submissions    │                                                                                
 └───────────────────┘                                                                                
                                                                                                      

------------------------------------------------------------------------------------------------------

Core Components                                                                                       

                                                                                                      
 Component      Technology                       Purpose                                              
 ──────────────────────────────────────────────────────────────────────────────────────────────────── 
 Backend        FastAPI (Python)                 REST API for LMS operations                          
 Database       PostgreSQL 18.3                  Persistent storage for learners, submissions, labs   
 Web Client     React                            Dashboard for viewing analytics                      
 Chat Client    Flutter (Web)                    WebSocket-based chat UI for nanobot                  
 Agent          Nanobot                          AI agent with MCP tools for natural-language queries 
 LLM Gateway    Qwen Code API                    Proxy to Qwen LLM for agent reasoning                
 Reverse Proxy  Caddy                            Routes traffic to all services                       
 Observability  OpenTelemetry + VictoriaMetrics  Logs and traces collection                           
                                                                                                      

------------------------------------------------------------------------------------------------------

Backend API Structure (/backend/app)                                                                  

                                                                                                      
 backend/app/                                                                                         
 ├── main.py           # FastAPI app entry point                                                      
 ├── auth.py           # API key verification                                                         
 ├── database.py       # DB connection                                                                
 ├── settings.py       # Configuration                                                                
 ├── etl.py            # Data pipeline logic                                                          
 ├── routers/                                                                                         
 │   ├── items.py      # Labs/tasks CRUD                                                              
 │   ├── learners.py   # Learner management                                                           
 │   ├── interactions.py # Student interactions                                                       
 │   ├── analytics.py  # Analytics endpoints                                                          
 │   └── pipeline.py   # Sync pipeline                                                                
 ├── models/                                                                                          
 │   ├── item.py       # Lab/task models                                                              
 │   ├── learner.py    # Learner models                                                               
 │   └── interaction.py # Interaction models                                                          
 └── data/                                                                                            
     └── init.sql      # Database initialization                                                      
                                                                                                      

API Endpoints:                                                                                        

 • GET/POST /items — Labs and tasks                                                                   
 • GET/POST /learners — Registered learners                                                           
 • GET/POST /interactions — Student submissions                                                       
 • GET /analytics/* — Pass rates, timelines, group performance                                        
 • POST /pipeline/sync — Trigger data sync                                                            

------------------------------------------------------------------------------------------------------

MCP Server (/mcp/mcp_lms)                                                                             

The Model Context Protocol (MCP) server exposes LMS operations as tools for the nanobot agent:        

                                                
 Tool                 Description               
 ────────────────────────────────────────────── 
 lms_health           Check backend health      
 lms_labs             List all labs             
 lms_learners         List all learners         
 lms_pass_rates       Get pass rates for a lab  
 lms_timeline         Get submission timeline   
 lms_groups           Get group performance     
 lms_top_learners     Get top learners by score 
 lms_completion_rate  Get completion rate       
 lms_sync_pipeline    Trigger sync pipeline     
                                                

------------------------------------------------------------------------------------------------------

Observability Stack                                                                                   

                                                           
 Component       Purpose                                   
 ───────────────────────────────────────────────────────── 
 OTel Collector  Receives traces/logs via OTLP (gRPC/HTTP) 
 VictoriaLogs    Log storage (7-day retention)             
 VictoriaTraces  Trace storage (7-day retention)           
                                                           

Data Flow:                                                                                            

                                                                                                      
 Backend (instrumented with OpenTelemetry)                                                            
     │                                                                                                
     ▼                                                                                                
 OTel Collector (port 4317/4318)                                                                      
     │                                                                                                
     ├──► VictoriaLogs (port 9428)                                                                    
     └──► VictoriaTraces (port 10428)                                                                 
                                                                                                      

------------------------------------------------------------------------------------------------------

Docker Compose Services                                                                               

                                               
 Service             Port          Network     
 ───────────────────────────────────────────── 
 backend             Configurable  lms-network 
 postgres            5432          lms-network 
 pgadmin             80            lms-network 
 caddy               Gateway port  lms-network 
 qwen-code-api       8080          lms-network 
 victorialogs        9428          lms-network 
 victoriatraces      10428         lms-network 
 otel-collector      4317/4318     lms-network 
 nanobot             (optional)    lms-network 
 client-web-flutter  (optional)    lms-network 
                                               

------------------------------------------------------------------------------------------------------

Key Design Patterns                                                                                   

 1 Agent-as-Interface — Nanobot provides natural-language access to LMS data via MCP tools            
 2 Multi-client Architecture — Same backend serves React dashboard, Flutter chat, and agent           
 3 Observability-first — All services emit OTLP traces/logs for debugging                             
 4 API Key Security — All endpoints protected by LMS_API_KEY                                          
 5 Containerized Deployment — All services run in Docker with lms-network isolation                   

This architecture enables users to query the LMS through multiple interfaces, with the AI agent       
providing the most flexible natural-language access pattern.                                          


## Task 1C — Skill prompt

**Question:** Show me the scores
**Answer:**
Here are the detailed scores for all labs:

**Lab 01 – Products, Architecture & Roles**
| Task | Avg Score | Attempts |
|------|-----------|----------|
| Lab setup | 84.2% | 2950 |
| Task 0: Practice the Git workflow | 64.5% | 306 |
| Task 1: Product & architecture description | 55.1% | 161 |
| Task 2: Roles and skills mapping | 49.8% | 83 |
| Optional: Create a release | 60.0% | 5 |
| Optional: Resolve a merge conflict | 34.6% | 13 |
| Optional: Add a CI check | 42.9% | 7 |
| Optional: Plan skill development | 20.0% | 5 |

**Lab 02 — Run, Fix, and Deploy a Backend Service**
| Task | Avg Score | Attempts |
|------|-----------|----------|
| Task 1: Run the web server | 67.9% | 210 |
| Task 2: Identify, report, and fix a bug | 66.7% | 216 |
| Task 4: Deploy the web server to the VM | 56.2% | 84 |
| Optional 3: Implement the post-order traversal | 62.5% | 6 |
| Optional 1: Implement the /outcomes endpoint | 36.4% | 11 |
| Optional 2: Make your VM a proxy | 0.0% | 3 |

**Lab 03 — Backend API: Explore, Debug, Implement, Deploy**
| Task | Avg Score | Attempts |
|------|-----------|----------|
| Task 1: Explore the API | 63.7% | 381 |
| Task 2: Enable and debug the interactions endpoint | 66.4% | 299 |
| Task 3: Implement the learners endpoint | 50.0% | 4 |
| Task 4: Deploy to a hardened VM | 43.9% | 110 |
| Optional 2: Set up CI with GitHub Actions | 44.0% | 5 |

**Lab 04 — Testing, Front-end, and AI Agents**
| Task | Avg Score | Attempts |
|------|-----------|----------|
| Task 3: Add Front-end | 68.8% | 4 |
| Task 1: Observe System Component Interaction | 60.9% | 686 |
| Task 2: Back-end Testing | 59.9% | 753 |
| Optional 1: CI/CD with GitHub Actions | 33.8% | 17 |

**Lab 05 — Data Pipeline and Analytics Dashboard**
| Task | Avg Score | Attempts |
|------|-----------|----------|
| Task 3: Dashboard Front-end | 74.1% | 21 |
| Task 1: Build the Data Pipeline | 73.5% | 626 |
| Task 2: Analytics Endpoints | 64.4% | 629 |
| Optional 1: Grafana Dashboard | 57.2% | 60 |

**Lab 06 — Build Your Own Agent**
| Task | Avg Score | Attempts |
|------|-----------|----------|
| Task 2: The Documentation Agent | 73.6% | 420 |
| Task 1: Call an LLM from Code | 61.1% | 680 |
| Task 3: The System Agent | 42.8% | 2051 |

**Lab 07 — Build a Client with an AI Coding Agent**
| Task | Avg Score | Attempts |
|------|-----------|----------|
| Task 1: Plan and Scaffold | 76.9% | 755 |
| Task 2: Backend Integration | 66.8% | 564 |
| Task 4: Containerize and Document | 66.5% | 678 |
| Task 3: Intent-Based Natural Language Routing | 58.9% | 733 |

**Lowest scoring tasks across all labs:**
1. Lab 02 — Optional 2: Make your VM a proxy: 0.0%
2. Lab 01 — Optional: Plan skill development: 20.0%
3. Lab 04 — Optional 1: CI/CD with GitHub Actions: 33.8%
4. Lab 02 — Optional 1: Implement the /outcomes endpoint: 36.4%
5. Lab 06 — Task 3: The System Agent: 42.8%
