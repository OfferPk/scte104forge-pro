# SCTE104Forge Pro (v1.0.0)

### ANSI/SCTE 104 & SMPTE ST 2010 Automation-to-Compression Ad Insertion Studio

---

## Overview

**SCTE104Forge Pro** is an operational broadcast engineering workbench, binary packet synthesizer, VANC inspector, and automated translation testing utility designed for television master control facilities, playout automation teams, ad-tech integration specialists, and live compression systems. It implements upstream SCTE 104 message generation, SMPTE ST 2010 SDI VANC encapsulation, and real-time translation into downstream ANSI/SCTE 35 binary cue payloads conforming to:

- **ANSI/SCTE 104 2022**: Automation System to Compression System Communications Applications Program Interface  
- **SMPTE ST 2010:2020**: Synchronization of SCTE 104 Messages into SDI Vertical Ancillary Data (VANC)  
- **ANSI/SCTE 35 2024**: Digital Program Insertion Cueing Message for Cable  
- **SMPTE ST 291-1**: Ancillary Data Packet and Space Formatting (DID 0x41, SDID 0x07)  
- **SMPTE ST 2110-40**: Professional Media Over Managed IP Networks: Ancillary Data

---

## Key Capabilities

1. **Visual SCTE 104 Message Builder & Splice Studio**:  
   - Visual configuration of splice parameters: Splice Event ID, Pre-Roll Time (500–10000ms), Break Duration (tenths of seconds), and auto-return flags.  
   - Generates Single Operation (`splice_request_data` 0x0101, `time_signal_request_data` 0x0104) and Multiple Operation (`0xFFFF`) message formats.  
   - Configures SCTE 104 Segmentation Descriptors (0x010B) across Provider Ad (0x30/0x31), Distributor Ad (0x34/0x35), Program Start/End (0x10/0x11), and DAI Placement Opportunities (0x36/0x37).  
2. **Binary Packet & VANC / SMPTE ST 2010 Inspector**:  
   - Byte-level hex dump of Single Operation (19-byte) and Multiple Operation wire packets.  
   - SMPTE ST 2010 SDI VANC packaging on Line 12 (DID 0x41, SDID 0x07) with 10-bit parity word formatting.  
   - TCP/IP automation socket dispatch generator (port 5167).  
3. **Upstream-to-Downstream SCTE-35 Translation Engine**:  
   - Real-time conversion of SCTE 104 operations into downstream ANSI/SCTE 35 binary wire messages (`table_id 0xFC`, `splice_insert` command 0x05, 90kHz PTS offset calculation, and MPEG-2 CRC-32 checksum).  
   - Generates copy-paste SCTE-35 Base64 cue strings (e.g. `/DA...`) and `.bin` downloads for direct testing in ad inserters and video players.  
4. **10-Point Normative Compliance Auditor**:  
   - Automated test harness validating operation IDs, pre-roll timing limits, break durations, segmentation descriptors, UPID formats, ST 2010 VANC framing, and SCTE 35 translation fidelity.  
5. **Production Python CLI (`scte104-cli.py`)**:  
   - Standalone CLI utility for broadcast CI/CD automation, supporting `build`, `vanc`, `translate`, and `audit` operations.

---

## Getting Started

### Web Studio

Open `source-code/index.html` in any web browser. Runs 100% locally with zero external network or cloud dependencies.

### Command-Line Interface

\# Build SCTE 104 binary message

python3 build/scte104-cli.py build \--event 1001 \--preroll 4000 \--duration 30.0

\# Encapsulate into SMPTE ST 2010 VANC

python3 build/scte104-cli.py vanc \--line 12

\# Translate SCTE 104 to downstream SCTE 35

python3 build/scte104-cli.py translate \--event 1001 \--duration 30.0 \--preroll 4000

\# Execute 10-point normative compliance audit

python3 build/scte104-cli.py audit

### Automated Testing

python3 test\_scte104.py