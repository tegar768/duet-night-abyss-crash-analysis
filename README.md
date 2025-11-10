# Jetsam Event Investigation for iOS

## Introduction 💡  
When I was playing *Duet Night Abyss* on my iPad 9, the game suddenly force-quit.  
At first, I thought it was a random bug — until I checked the system log and found a **Jetsam Event**.  

This repository documents my systematic investigation into why this happened and how iOS memory management works.

---

## What I Discovered

### The Core Issue
After analyzing system logs and memory patterns, I found that the game wasn't crashing due to a bug - it was being intentionally terminated by iOS's Jetsam process because of memory pressure.

### My Investigation Approach
1. **Collected Evidence**: Extracted and analyzed Jetsam event logs
2. **Pattern Recognition**: Identified memory usage trends leading to termination  
3. **Root Cause Analysis**: Correlated game events with system behavior
4. **Platform Understanding**: Researched iOS memory management mechanisms

---

## Technical Breakdown

### What is Jetsam?
Jetsam is iOS's internal memory manager that:
- Monitors system-wide memory usage
- Terminates processes when memory gets critically low
- Maintains overall system stability
- Generates detailed `.ips` log files for analysis

### Memory Pressure States
| Level | System Behavior | What User Experiences |
|-------|-----------------|----------------------|
| **Normal** | Plenty of free memory | Smooth performance |
| **Warning** | Compresses inactive memory | Minor stuttering |
| **Critical** | Terminates processes | Apps force-quit |

### The Specific Case
**Device**: iPad 9th Gen (3GB RAM)  
**Game**: Duet Night Abyss  
**Trigger**: Memory-intensive scenes (boss battles, complex rendering)  
**Result**: Jetsam termination with `per-process-limit` reason

---

## Investigation Methodology

### Step 1: Log Collection
```bash
# Approach for extracting system logs
log show --predicate 'process == "jetsam"' --last 24h > investigation_logs.txt
