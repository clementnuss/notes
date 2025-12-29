# CKS (Certified Kubernetes Security Specialist) Study Guide

> **Comprehensive study guide based on killer.sh exam simulations and real exam scenarios**

---

## Table of Contents

1. [Exam Overview](#exam-overview)
2. [Exam Environment & Technical Setup](#exam-environment--technical-setup)
3. [Network Policies](#network-policies)
4. [RBAC (Role-Based Access Control)](#rbac-role-based-access-control)
5. [AppArmor](#apparmor)
6. [Trivy & Vulnerability Scanning](#trivy--vulnerability-scanning)
7. [Service Security](#service-security)
8. [ServiceAccount Token Management](#serviceaccount-token-management)
9. [gVisor & RuntimeClass](#gvisor--runtimeclass)
10. [Falco Runtime Security](#falco-runtime-security)
11. [SBOM (Software Bill of Materials)](#sbom-software-bill-of-materials)
12. [Runtime Security Detection](#runtime-security-detection)
13. [Exam Tips & Tricks](#exam-tips--tricks)
14. [Quick Reference](#quick-reference)

---

## Exam Overview

### CKS Curriculum Breakdown

| Domain                                 | Weight |
| -------------------------------------- | ------ |
| Cluster Setup & Hardening              | 20%    |
| System Hardening                       | 15%    |
| Minimize Microservice Vulnerabilities  | 20%    |
| Supply Chain Security                  | 20%    |
| Monitoring, Logging & Runtime Security | 20%    |

### Key Tools You Must Know

- **Falco** - Runtime threat detection
- **Trivy** - Vulnerability & SBOM scanning
- **AppArmor** - Mandatory Access Control
- **Seccomp** - System call filtering
- **gVisor/runsc** - Sandboxed container runtime
- **bom** - SBOM generation for Kubernetes
- **kubectl** - You should be extremely proficient

### Essential Resources

- Curriculum: <https://github.com/cncf/curriculum>
- Kubernetes docs: <https://kubernetes.io/docs/>
- Killercoda CKS scenarios: <https://killercoda.com/killer-shell-cks>
- Walid Shaari's repo: <https://github.com/walidshaari/Certified-Kubernetes-Security-Specialist>
- Cloud Native Security Whitepaper

---

## Exam Environment & Technical Setup

### ⚠️ CRITICAL: PSI Secure Browser Issues

The CKS exam uses **PSI Secure Browser** - a locked-down testing environment that is **extremely sensitive to background processes and services**. Many candidates have experienced significant technical difficulties, especially on macOS.

### Real Experience: macOS Nightmare

**What happened:**

- 2+ hours of pain trying to get into the exam session on MacBook Air (MacOS 15.1)
- PSI Secure Browser repeatedly crashing
- Background services causing conflicts (e.g., `photolibraryserviced`)
- Despite preparation: fresh reboot, Bluetooth off, new empty user without iCloud
- **Finally worked** after unplugging laptop (possibly due to energy saving disabling background services)

### ⭐ STRONGLY RECOMMENDED: Use Ubuntu

**If you had to do it again, the recommendation is clear:**

```bash
# Install a clean Ubuntu installation
# Either dual-boot or use a separate machine
# This avoids macOS/Windows background service conflicts
```

**Why Ubuntu?**

- ✅ Fewer background services by default
- ✅ Better control over what's running
- ✅ More stable exam experience

### Pre-Exam Technical Checklist

#### At Least 1 Week Before

```bash
# 1. Test PSI Secure Browser compatibility
# Download from: https://syscheck.bridge.psiexams.com/
# Run system compatibility check.
# Let the "fake exame" browser environment for 1 hour, to make sure it doesn't crash!

# 2. If on macOS/Windows - consider Ubuntu setup
# Option A: Dual boot Ubuntu
# Option B: Dedicated Ubuntu laptop (or live CD)
# Option C: Ubuntu VM (may not be allowed - check with PSI)
```

#### 24 Hours Before Exam

```bash
# On Ubuntu (recommended)
sudo apt update && sudo apt upgrade -y
sudo systemctl disable bluetooth
sudo systemctl stop bluetooth

# Close unnecessary applications
# Clear desktop - exam proctor will check
# Test internet connection
# Test webcam and microphone
```

#### On macOS (if you must)

```bash
# 1. Create a new user account WITHOUT iCloud
# System Preferences → Users & Groups → Add User
# DO NOT sign into iCloud, App Store, etc.

# 2. Restart into new user

# 3. Disable problematic services
# Close: Photos, iCloud Drive, Dropbox, OneDrive, etc.

# 4. Disable Bluetooth
# Turn off in System Preferences

# 5. Quit ALL applications
# Leave only Finder running

# 6. Close notification center

# 7. **Try unplugging power** (based on real experience)
# This may disable energy-saving background services

# 8. Disable Spotlight indexing temporarily
sudo mdutil -a -i off
```

#### On Windows (if you must)

```bash
# 1. Close ALL applications
# Check Task Manager for background processes

# 2. Disable Windows Defender temporarily (may be required)
# Windows Security → Virus & threat protection → Manage settings

# 3. Disable Windows Update
# Settings → Windows Update → Pause updates

# 4. Close OneDrive, Dropbox, etc.

# 5. Disable notifications
# Settings → System → Notifications
```

### Day of Exam

```bash
# 1. Reboot your machine fresh
sudo reboot

# 2. Don't open ANYTHING except PSI Secure Browser
# No browser, no terminal, no Slack, nothing

# 3. Clear your desk
# Proctor will ask you to show your workspace via webcam
# Remove: papers, books, second monitors, phones, drinks (usually)

# 4. Have government ID ready
# Physical ID, not digital

# 5. Check-in opens 30 minutes before exam
# Don't be late. You can only download the PSI Secure Browser (link to the exam) up to 30mins after your time window begins.

# 6. Be patient during check-in
# Proctor may take 5-15 minutes to connect
# They'll ask to see room, desk, ID, etc.
```

### Known PSI Secure Browser Issues

#### macOS Specific

**Services that cause crashes:**

- `photolibraryserviced` - iCloud Photos background sync
- `cloudd` - iCloud Drive sync
- `bird` - iCloud background service
- `Dropbox`, `OneDrive` - Cloud storage sync
- `Spotlight` indexing
- `Time Machine` backups
- `Bluetooth` services
- Various Apple background sync services

**Workarounds:**

```bash
# Kill problematic processes (may respawn)
killall photolibraryserviced
killall cloudd
killall bird

# Disable iCloud Drive (System Preferences)
# Disable Bluetooth (System Preferences)
# Sign out of iCloud completely (for exam user)

# Nuclear option: Unplug laptop from power
# (Based on real experience - may disable aggressive background services)
```

#### Windows Specific

**Services that cause issues:**

- Windows Defender (may block PSI browser)
- Windows Update (background downloads)
- OneDrive sync
- Antivirus software
- VPN software
- Background game services (Steam, Epic, etc.)

#### Linux/Ubuntu (Recommended - Fewer Issues!)

**Rarely problematic, but check:**

```bash
# Verify no package updates running
ps aux | grep -i apt

# Stop unnecessary services
sudo systemctl stop bluetooth
sudo systemctl stop cups  # Printing service

# Check what's running
systemctl list-units --type=service --state=running
```

### Exam Environment (Once You're In)

**What you get:**

- Remote desktop to Ubuntu-based exam environment
- Pre-installed: kubectl, vim etc.
- Access to one browser tab: <https://kubernetes.io/docs/>
- Cannot copy-paste from your local machine (in most cases)

**Clipboard:**

- You CAN copy-paste within the exam environment
- Ctrl+C/Ctrl+V works inside the remote desktop

**Terminal:**

- Multiple terminals available

### Troubleshooting During Check-In

**If PSI Secure Browser crashes:**

1. **Close everything** on your system
2. **Reboot** your machine
3. **Don't open anything** except PSI Browser
4. **Try unplugging laptop** from power (if on battery)
5. **Contact PSI support** via phone (number in exam confirmation email)
6. **Request rescheduling** if technical issues persist (usually allowed)

**If you can't reschedule same-day:**

- PSI support can usually give you a new exam session
- May be same day or next available
- Don't panic - technical issues are common

### Final Recommendations

**BEST SETUP (in order of preference):**

1. ✅ **Blank Ubuntu installation** (dual boot or dedicated machine)
2. ✅ **Ubuntu on older laptop** you can wipe clean
3. ⚠️ **macOS with new user + all services disabled + unplugged**
4. ⚠️ **Windows with all background services disabled**

**AVOID:**

- ❌ macOS with iCloud enabled
- ❌ Any system with active background sync (Dropbox, OneDrive, etc.)
- ❌ Systems with aggressive antivirus
- ❌ VMs (may not be allowed by PSI)

### The Bottom Line

**Invest time in your exam environment setup.** A clean Ubuntu installation takes 30 minutes but can save you hours of frustration and potentially a failed exam due to technical issues.

The exam is hard enough - don't let PSI Secure Browser be the reason you struggle.

---

## Network Policies

### Core Concept

**NetworkPolicies are ADDITIVE (OR logic)** - multiple policies combine, they don't override each other.

### Default Behavior

```yaml
# NO NetworkPolicy = Allow ALL traffic (non-isolated)
# Empty NetworkPolicy = Deny ALL traffic (isolated)
```

### Common Pattern: Default Deny + Explicit Allow

```yaml
# 1. Default deny all ingress
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-ingress
  namespace: production
spec:
  podSelector: {} # Selects ALL pods
  policyTypes:
    - Ingress
  # No ingress rules = deny all
---
# 2. Default deny all egress
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-egress
  namespace: production
spec:
  podSelector: {}
  policyTypes:
    - Egress
  # No egress rules = deny all
---
# 3. Allow specific traffic
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend-to-backend
  namespace: production
spec:
  podSelector:
    matchLabels:
      app: backend
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: frontend
      ports:
        - protocol: TCP
          port: 8080
```

### Common Allows You'll Need

#### Allow DNS (Always Needed)

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-dns
spec:
  podSelector: {}
  policyTypes:
    - Egress
  egress:
    - to:
        - namespaceSelector:
            matchLabels:
              name: kube-system
      ports:
        - protocol: UDP
          port: 53
```

#### Allow Namespace-to-Namespace

```yaml
egress:
  - to:
      - namespaceSelector:
          matchLabels:
            name: target-namespace
```

### Key Points

✅ NetworkPolicies are **additive** (union/OR logic)
✅ No explicit "deny" rules - only allows
✅ Everything not allowed is denied
✅ Policies are namespace-scoped
✅ Requires CNI plugin support (Calico, Cilium, etc.)

### Common Exam Tasks

- Create default deny policies
- Allow specific pod-to-pod communication
- Allow egress to specific namespaces
- Troubleshoot connectivity issues

---

## RBAC (Role-Based Access Control)

### Key Resources

- **Role** - Namespace-scoped permissions
- **ClusterRole** - Cluster-scoped permissions
- **RoleBinding** - Binds Role to subjects (namespace-scoped)
- **ClusterRoleBinding** - Binds ClusterRole to subjects (cluster-scoped)

### Important Rules

⚠️ **Common Mistake:** A `RoleBinding` can reference a `ClusterRole`, but grants permissions only within that namespace.

```yaml
# WRONG - This won't work as expected
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: k97
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: k97
roleRef:
  kind: ClusterRole # ← References ClusterRole
  name: pod-reader # ← But pod-reader is a Role!
  apiGroup: rbac.authorization.k8s.io
```

### Correct Patterns

```yaml
# Pattern 1: Role + RoleBinding (namespace-scoped)
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: default
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
  - kind: ServiceAccount
    name: my-sa
    namespace: default
roleRef:
  kind: Role # ← Matches the Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

```yaml
# Pattern 2: ClusterRole + RoleBinding (namespace-scoped permissions)
# Use existing ClusterRole but limit to namespace
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
  - kind: ServiceAccount
    name: my-sa
roleRef:
  kind: ClusterRole # ← Can reference ClusterRole
  name: view # ← Built-in ClusterRole
  apiGroup: rbac.authorization.k8s.io
```

### Common Commands

```bash
# Check permissions
kubectl auth can-i <verb> <resource> --as=system:serviceaccount:<ns>:<sa>
kubectl auth can-i list pods --as=system:serviceaccount:default:my-sa

# View roles and bindings
kubectl get roles,rolebindings -n <namespace>
kubectl get clusterroles,clusterrolebindings

# Describe to see details
kubectl describe role <name> -n <namespace>
kubectl describe rolebinding <name> -n <namespace>
```

---

## AppArmor

### What is AppArmor?

**AppArmor** is a Linux kernel security module that restricts what programs can do using per-program profiles.

### Key Concepts

- **Whitelist-based** - You define what's allowed, everything else is denied
- **Profile** - Set of rules for a program
- **Modes:**
  - **Enforce** - Blocks violations
  - **Complain** - Logs violations but doesn't block

### Profile Structure

```
#include <tunables/global>

profile <profile-name> flags=(attach_disconnected) {
  #include <abstractions/base>

  # Rules go here
}
```

### Network Rules

```
# Allow all network
network,

# Deny all network
deny network,

# Allow specific protocols
network inet stream,      # TCP IPv4
network inet dgram,       # UDP IPv4
network inet raw,         # Raw sockets (ping)
network inet6 stream,     # TCP IPv6
```

### File Execution Modes

- **`ix`** - Inherit execute (child inherits parent's profile)
- **`px`** - Profile execute (child must have its own profile)
- **`ux`** - Unconfined execute (child runs without AppArmor)
- **`cx`** - Child execute (transition to subprofile)
- **Uppercase (Px, Ux, Cx)** - Same but scrubs environment variables (safer)

### Example: Block Network Access

```
#include <tunables/global>

profile network-deny flags=(attach_disconnected) {
  #include <abstractions/base>

  # Allow executing programs
  /bin/** ix,
  /usr/bin/** ix,

  # Deny all network
  deny network,
}
```

### Loading Profiles

```bash
# 1. Create profile
sudo vim /etc/apparmor.d/network-deny

# 2. Load profile
sudo apparmor_parser /etc/apparmor.d/network-deny

# 3. Verify it's loaded
sudo aa-status | grep network-deny
```

### Using in Kubernetes

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secured-pod
spec:
  securityContext:
    appArmorProfile:
      type: Localhost
      localhostProfile: network-deny # Profile name
  containers:
    - name: app
      image: nginx
```

### Key Points

✅ `attach_disconnected` flag is **required** for containers
✅ Profiles are **node-level** (not Kubernetes resources)
✅ Cannot modify running pods - must delete and recreate
✅ Use `complain` mode for testing profiles
✅ `abstractions/base` provides essential file access

---

## Trivy & Vulnerability Scanning

### What is Trivy?

Open-source vulnerability scanner for:

- Container images
- Filesystems
- Git repositories
- Kubernetes manifests
- SBOMs

### Common Commands

```bash
# Scan an image
trivy image <image-name>

# Scan only HIGH and CRITICAL severities
trivy image --severity HIGH,CRITICAL <image>

# Output as JSON
trivy image --format json --output result.json <image>

# Scan for specific CVEs
trivy image <image> | grep -E "CVE-2020-1234|CVE-2020-5678"

# Scan local filesystem
trivy fs /path/to/project

# Scan Kubernetes manifest
trivy config deployment.yaml
```

### Exam Pattern: Find Images Without Specific CVEs

**Task:** Write images that DON'T contain CVE-2020-10878 or CVE-2020-1967 to a file.

**Solution:**

```bash
# Method 1: Manual check
for img in "nginx:1.16.1-alpine" "k8s.gcr.io/kube-apiserver:v1.18.0"; do
  if ! trivy image "$img" 2>/dev/null | grep -qE "CVE-2020-10878|CVE-2020-1967"; then
    echo "$img"
  fi
done > /opt/course/2/good-images

# Method 2: Check exit code
trivy image nginx:1.16.1-alpine | grep -E "CVE-2020-10878|CVE-2020-1967"
echo $?  # 1 = no match (good), 0 = found (bad)
```

### Key Points

✅ Trivy can scan images, SBOMs, configs, filesystems
✅ Use `--severity` to filter results
✅ Can output JSON, table, SARIF formats
✅ Scans can take 30-60 seconds - be patient

---

## Service Security

### Service Types & Security

- **ClusterIP** - Internal only (most secure) ✅
- **NodePort** - Exposed on node IPs (less secure)
- **LoadBalancer** - External load balancer (least secure)

### Common Task: Change NodePort to ClusterIP

**Problem:** API server exposed via NodePort (accessible from outside cluster)

**Solution:**

```bash
# Option 1: Edit directly
kubectl edit svc kubernetes -n default

# Change:
#   type: NodePort
# To:
#   type: ClusterIP
# Remove nodePort field

# Option 2: Patch
kubectl patch svc kubernetes -n default -p '{"spec":{"type":"ClusterIP"}}'

# Verify
kubectl get svc kubernetes -n default
# Should show TYPE=ClusterIP
```

### Why It Matters

- NodePort exposes services on all nodes' external IPs
- API server on NodePort = external attack surface
- ClusterIP = only reachable within cluster = defense in depth

---

## ServiceAccount Token Management

### Security Best Practices

1. **Disable automounting** if container doesn't need API access
2. **Use short-lived tokens** (expiration)
3. **Mount at custom paths** for better control
4. **Use projected volumes** for advanced token configuration

### Complete Example

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secure-pod
  annotations:
    token-lifetime: "1200" # Annotation for documentation
spec:
  serviceAccountName: my-sa
  automountServiceAccountToken: false # Disable default mount

  containers:
    - name: app
      image: nginx
      volumeMounts:
        - name: custom-token
          mountPath: /var/run/secrets/custom/
          readOnly: true

  volumes:
    - name: custom-token
      projected:
        sources:
          - serviceAccountToken:
              path: token # Creates /var/run/secrets/custom/token
              expirationSeconds: 1200 # 20 minutes
              audience: api # Optional: token audience
```

### Key Points

✅ Default token path: `/var/run/secrets/kubernetes.io/serviceaccount/token`
✅ `automountServiceAccountToken: false` prevents default mounting
✅ Use `projected` volume type for custom tokens
✅ `expirationSeconds` creates short-lived tokens (better security)
✅ Annotation goes in **pod template**, not Deployment metadata

### Verification

```bash
# Check old default path is empty
kubectl exec <pod> -- ls /var/run/secrets/kubernetes.io/serviceaccount/
# Should be empty or not exist

# Check custom path has token
kubectl exec <pod> -- ls /var/run/secrets/custom/
# Should show: token

# View token details
kubectl exec <pod> -- cat /var/run/secrets/custom/token
```

---

## gVisor & RuntimeClass

### What is gVisor?

**gVisor** is a sandboxed container runtime that provides stronger isolation by running containers in a user-space kernel.

### Security Benefits

- Containers don't directly access host kernel
- Syscalls intercepted and filtered by gVisor
- Reduced attack surface
- Defense against kernel exploits

### RuntimeClass Setup

```yaml
# 1. Create RuntimeClass (cluster-scoped)
apiVersion: node.k8s.io/v1
kind: RuntimeClass
metadata:
  name: gvisor
handler: runsc # Runtime handler (must be installed on node)
```

### Using RuntimeClass in Pods

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secure-pod
  namespace: team-purple
spec:
  runtimeClassName: gvisor # Use gVisor runtime
  nodeName: worker-node-1 # Pin to specific node (if needed)
  containers:
    - name: app
      image: nginx:1-alpine
```

### Node Scheduling

```yaml
# Option 1: nodeName (hard constraint - simplest)
spec:
  nodeName: worker-node-1

# Option 2: nodeSelector (label-based)
spec:
  nodeSelector:
    kubernetes.io/hostname: worker-node-1

# Option 3: nodeAffinity (advanced)
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: kubernetes.io/hostname
            operator: In
            values:
            - worker-node-1
```

### Verification

```bash
# Check pod is using gVisor
kubectl get pod -o yaml | grep runtimeClassName

# Compare dmesg output (should be different from host)
kubectl exec <pod> -- dmesg | head
# Should show "gVisor" or different kernel messages

# Check which node it's running on
kubectl get pod -o wide
```

### Key Points

✅ RuntimeClass is cluster-scoped (no namespace)
✅ `handler` must match installed runtime on nodes
✅ Use `nodeName` to pin pods to specific nodes
✅ gVisor has slight performance overhead but much better isolation
✅ `dmesg` output will differ from host (proves sandboxing)

---

## Falco Runtime Security

### What is Falco?

**Falco** monitors system calls and detects unexpected behavior based on rules you define.

### Rule Structure

```yaml
- rule: Rule Name
  desc: Description of what this detects
  condition: >
    (boolean logic that triggers the rule)
  output: "Log message with %variables"
  priority: WARNING
```

### Common Filters

#### File Access

- `fd.name` - File path being accessed
- `fd.directory` - Directory containing file
- `open_read` - File opened for reading (macro)
- `open_write` - File opened for writing (macro)

#### Process

- `proc.name` - Process name
- `proc.cmdline` - Full command line
- `spawned_process` - New process started (macro)

#### System Calls

- `evt.type` - System call type (open, kill, execve, etc.)
- `evt.arg.sig` - Signal number (for kill syscall)
- `evt.arg.pid` - PID argument

#### Container

- `container` - Boolean: true if event from container
- `container.id` - Container ID
- `container.name` - Container name

### Example Rules

#### Detect /etc/kubernetes File Access

```yaml
- rule: Custom Rule 1
  desc: Detect containers accessing /etc/kubernetes files
  condition: >
    container and
    (open_read or open_write) and
    fd.name startswith "/etc/kubernetes"
  output: "custom_rule_1 file=%fd.name container=%container.id"
  priority: WARNING
```

#### Detect kill Syscalls

```yaml
- rule: Custom Rule 2
  desc: Detect kill syscalls
  condition: >
    evt.type = kill
  output: "custom_rule_2 event_signal=%evt.arg.sig event_pid=%evt.arg.pid container=%container.id"
  priority: INFO
```

### Working with Falco

```bash
# Validate rules
sudo falco --validate /etc/falco/falco_rules.local.yaml

# Run Falco
sudo falco -c /etc/falco/falco.yaml

# Run for specific duration and save logs
sudo timeout 30 falco -c /etc/falco/falco.yaml > /opt/course/16/logs 2>&1

# View Falco service logs
sudo journalctl -u falco -f

# Check loaded rules
sudo falco --list
```

### Custom Rules Location

```bash
# Edit custom rules file
sudo vim /etc/falco/falco_rules.local.yaml

# Or check main config
cat /etc/falco/falco.yaml | grep rules_file
```

### Key Points

✅ Rules are **whitelist-based** - define what to detect
✅ Output format: use `%variable` not `{{variable}}`
✅ Priority levels: EMERGENCY > ALERT > CRITICAL > ERROR > WARNING > NOTICE > INFO > DEBUG
✅ Test rules in **complain mode** first
✅ `abstractions/nameservice` can override deny rules

---

## SBOM (Software Bill of Materials)

### What is an SBOM?

An "ingredients list" for software - documents all components, libraries, and dependencies.

### SBOM Formats

- **SPDX** (Software Package Data Exchange) - Linux Foundation standard
- **CycloneDX** - OWASP standard, security-focused

### Tools

- **bom** - Kubernetes SIG tool (generates SPDX)
- **trivy** - Can generate both SPDX and CycloneDX

### Generate SBOM with bom (SPDX)

```bash
# Generate SPDX-JSON SBOM
bom generate \
  -o /opt/course/1/sbom1.json \
  --format json \
  --image registry.k8s.io/kube-apiserver:v1.31.0

# Verify
head /opt/course/1/sbom1.json
# Should see: "spdxVersion": "SPDX-2.3"
```

### Generate SBOM with trivy (CycloneDX)

```bash
# Generate CycloneDX SBOM
trivy image \
  --format cyclonedx \
  --output /opt/course/1/sbom2.json \
  registry.k8s.io/kube-controller-manager:v1.31.0

# Verify
head /opt/course/1/sbom2.json
# Should see: "bomFormat": "CycloneDX"
```

### Scan Existing SBOM for Vulnerabilities

```bash
# Scan an SBOM file (not an image)
trivy sbom \
  --format json \
  --output /opt/course/1/sbom_check_result.json \
  /opt/course/1/sbom_check.json

# Verify
cat /opt/course/1/sbom_check_result.json | jq .
```

### Key Points

✅ **bom** generates SPDX format
✅ **trivy** can generate both SPDX and CycloneDX
✅ Use `trivy sbom` to scan SBOM files (not `trivy image`)
✅ SBOM generation can take time - be patient
✅ Part of **Supply Chain Security** (20% of exam)

---

## Runtime Security Detection

### Common Scenario: Detect Suspicious File Access

**Example:** Find which pod is accessing `/dev/mem` (direct physical memory access - huge security risk!)

### Detection Methods

#### Method 1: Falco Logs (Best Option)

```bash
# Check Falco logs
sudo journalctl -u falco | grep -i "/dev/mem"

# Look for pod name in output
# Example: "pod=suspicious-pod container=attacker file=/dev/mem"

# Extract pod name
sudo journalctl -u falco | grep -i "/dev/mem" | grep -oP 'pod=\K[^ ]+'
```

#### Method 2: Find Privileged Pods

```bash
# Privileged pods can access /dev/mem
kubectl get pods -A -o json | \
  jq -r '.items[] |
  select(.spec.containers[].securityContext.privileged == true) |
  .metadata.namespace + "/" + .metadata.name'
```

#### Method 3: Find hostPath /dev Mounts

```bash
# Pods mounting /dev from host
kubectl get pods -A -o json | \
  jq -r '.items[] |
  select(.spec.volumes[]?.hostPath.path | strings | startswith("/dev")) |
  .metadata.namespace + "/" + .metadata.name'
```

#### Method 4: Check SYS_RAWIO Capability

```bash
# This capability allows raw I/O operations
kubectl get pods -A -o json | \
  jq -r '.items[] |
  select(.spec.containers[].securityContext.capabilities.add[]? == "SYS_RAWIO") |
  .metadata.namespace + "/" + .metadata.name'
```

#### Method 5: Search Pod Manifests

```bash
# Direct YAML search
kubectl get pods -A -o yaml | grep -B 20 "/dev/mem"
```

#### Method 6: Audit Logs

```bash
# Search system audit logs
sudo ausearch -f /dev/mem

# Or grep directly
sudo grep "/dev/mem" /var/log/audit/audit.log
```

#### Method 7: lsof on Node

```bash
# SSH to node
ssh worker-node

# Check what has /dev/mem open
sudo lsof | grep /dev/mem

# Filter for container processes
sudo lsof | grep /dev/mem | grep containerd
```

### Complete Detection Script

```bash
#!/bin/bash

echo "=== Checking Falco Logs ==="
sudo journalctl -u falco 2>/dev/null | grep -i "/dev/mem" | tail -5

echo -e "\n=== Privileged Pods ==="
kubectl get pods -A -o json | jq -r '.items[] | select(.spec.containers[].securityContext.privileged == true) | .metadata.namespace + "/" + .metadata.name'

echo -e "\n=== /dev hostPath Mounts ==="
kubectl get pods -A -o json | jq -r '.items[] | select(.spec.volumes[]?.hostPath.path | strings | startswith("/dev")) | .metadata.namespace + "/" + .metadata.name'

echo -e "\n=== SYS_RAWIO Capability ==="
kubectl get pods -A -o json | jq -r '.items[] | select(.spec.containers[].securityContext.capabilities.add[]? == "SYS_RAWIO") | .metadata.namespace + "/" + .metadata.name'
```

### Key Points

✅ **Falco** is your first choice for runtime detection
✅ Check for **privileged pods**, **hostPath mounts**, **dangerous capabilities**
✅ Use **audit logs** if Falco isn't available
✅ Know where logs are stored: `/var/log/falco/`, `journalctl -u falco`, `/var/log/audit/`
✅ Multiple detection methods - try several in the exam

---

## Exam Tips & Tricks

### Speed & Efficiency

#### Essential Aliases

```bash
# Add to ~/.bashrc or run at start of exam
alias k=kubectl
alias kgp="kubectl get pods"
alias kgs="kubectl get svc"
alias kgn="kubectl get nodes"
alias kd="kubectl describe"
alias kdel="kubectl delete"
export do="--dry-run=client -o yaml"
export now="--force --grace-period=0"
```

#### Quick YAML Generation

```bash
# Pod
kubectl run <name> --image=<image> $do > pod.yaml

# Deployment
kubectl create deployment <name> --image=<image> $do > deploy.yaml

# Service
kubectl expose deployment <name> --port=80 $do > svc.yaml

# ConfigMap
kubectl create configmap <name> --from-literal=key=value $do > cm.yaml

# Secret
kubectl create secret generic <name> --from-literal=key=value $do > secret.yaml

# ServiceAccount
kubectl create serviceaccount <name> $do > sa.yaml

# NetworkPolicy (use kubectl.kubernetes.io for examples)
```

#### Fast Editing

```bash
# Edit directly
kubectl edit <resource> <name>

# Set image quickly
kubectl set image deployment/<name> <container>=<new-image>

# Scale quickly
kubectl scale deployment/<name> --replicas=3

# Restart deployment
kubectl rollout restart deployment/<name>
```

### Context Switching

```bash
# View contexts
kubectl config get-contexts

# Switch context
kubectl config use-context <context-name>

# Set namespace for context
kubectl config set-context --current --namespace=<namespace>
```

### Documentation Bookmarks

**Save these in your exam browser:**

- <https://kubernetes.io/docs/>
- <https://kubernetes.io/docs/reference/kubectl/>
- <https://kubernetes.io/docs/concepts/security/>
- <https://falco.org/docs/>

### Time Management

- **2 hours for ~15-20 questions**
- Spend ~5-7 minutes per question
- **Flag difficult questions** and come back
- Easy wins first - build confidence
- **Last 15 minutes:** Review flagged questions

### Common Pitfalls

❌ Editing Deployment instead of Pod template
❌ Forgetting to switch context/namespace
❌ Not verifying changes (always check your work!)
❌ Spending too long on one question
❌ Not reading the question carefully (which namespace? which node?)
❌ Typos in labels, names, paths

### Verification Checklist

After each task:

```bash
# 1. Check resource exists
kubectl get <resource> <name> -n <namespace>

# 2. Verify it's in correct state
kubectl get <resource> <name> -n <namespace> -o yaml

# 3. Check logs if applicable
kubectl logs <pod> -n <namespace>

# 4. Test functionality
kubectl exec <pod> -- <command>

# 5. Verify file outputs
ls -lh /opt/course/X/
cat /opt/course/X/answer.txt
```

---

## Quick Reference

### SecurityContext Common Settings

```yaml
# Pod-level
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
    runAsNonRoot: true
    seccompProfile:
      type: RuntimeDefault

# Container-level
containers:
  - name: app
    securityContext:
      allowPrivilegeEscalation: false
      readOnlyRootFilesystem: true
      runAsNonRoot: true
      capabilities:
        drop:
          - ALL
        add:
          - NET_BIND_SERVICE
```

### Admission Controllers

```bash
# View enabled admission controllers
kubectl exec -n kube-system kube-apiserver-<node> -- kube-apiserver -h | grep enable-admission-plugins

# Enable in kube-apiserver
# Edit: /etc/kubernetes/manifests/kube-apiserver.yaml
--enable-admission-plugins=NodeRestriction,PodSecurityPolicy,ImagePolicyWebhook
```

### Pod Security Standards

```yaml
# Namespace labels
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace
  labels:
    pod-security.kubernetes.io/enforce: restricted
    pod-security.kubernetes.io/audit: restricted
    pod-security.kubernetes.io/warn: restricted
```

Levels: `privileged` < `baseline` < `restricted`

### Seccomp Profiles

```yaml
spec:
  securityContext:
    seccompProfile:
      type: RuntimeDefault # or Localhost
      localhostProfile: profiles/audit.json
```

### Common File Locations

```bash
# Kubernetes configs
/etc/kubernetes/manifests/              # Static pod manifests
/etc/kubernetes/pki/                    # Certificates
/var/lib/kubelet/config.yaml            # Kubelet config

# Security tools
/etc/apparmor.d/                        # AppArmor profiles
/etc/falco/                             # Falco rules
/var/log/falco/                         # Falco logs
/var/log/audit/audit.log                # System audit log

# Container runtime
/etc/containerd/config.toml             # containerd config
/var/lib/containerd/                    # containerd data
```

### Useful Commands

```bash
# Check node security
kubectl get nodes -o json | jq '.items[].status.nodeInfo'

# Drain node
kubectl drain <node> --ignore-daemonsets --delete-emptydir-data

# Uncordon node
kubectl uncordon <node>

# View events
kubectl get events -A --sort-by=.metadata.creationTimestamp

# Check API resources
kubectl api-resources

# Explain resource fields
kubectl explain pod.spec.securityContext
```

---

## Practice Scenarios

### Scenario 1: Secure a Deployment

**Task:** Secure the deployment `web-app` in namespace `production`:

- Run as non-root user (UID 1000)
- Read-only root filesystem
- Drop all capabilities
- Use RuntimeDefault seccomp

<details>
<summary>Solution</summary>

```bash
kubectl edit deployment web-app -n production
```

Add to pod template:

```yaml
spec:
  template:
    spec:
      securityContext:
        runAsUser: 1000
        runAsNonRoot: true
        seccompProfile:
          type: RuntimeDefault
      containers:
        - name: web
          securityContext:
            allowPrivilegeEscalation: false
            readOnlyRootFilesystem: true
            capabilities:
              drop:
                - ALL
```

</details>

### Scenario 2: Create Network Isolation

**Task:** In namespace `backend`:

- Deny all ingress/egress by default
- Allow ingress from namespace `frontend` on port 8080
- Allow egress to kube-system for DNS

<details>
<summary>Solution</summary>

```yaml
# default-deny.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny
  namespace: backend
spec:
  podSelector: {}
  policyTypes:
    - Ingress
    - Egress
---
# allow-from-frontend.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-from-frontend
  namespace: backend
spec:
  podSelector: {}
  policyTypes:
    - Ingress
  ingress:
    - from:
        - namespaceSelector:
            matchLabels:
              name: frontend
      ports:
        - protocol: TCP
          port: 8080
---
# allow-dns.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-dns
  namespace: backend
spec:
  podSelector: {}
  policyTypes:
    - Egress
  egress:
    - to:
        - namespaceSelector:
            matchLabels:
              name: kube-system
      ports:
        - protocol: UDP
          port: 53
```

</details>

### Scenario 3: Custom ServiceAccount Tokens

**Task:** Create pod `api-client` using ServiceAccount `api-sa`:

- Disable default token automounting
- Mount custom token at `/var/run/secrets/custom/` with 1-hour expiration

<details>
<summary>Solution</summary>

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: api-client
spec:
  serviceAccountName: api-sa
  automountServiceAccountToken: false
  containers:
    - name: client
      image: curlimages/curl:latest
      command: ["sleep", "3600"]
      volumeMounts:
        - name: custom-token
          mountPath: /var/run/secrets/custom/
          readOnly: true
  volumes:
    - name: custom-token
      projected:
        sources:
          - serviceAccountToken:
              path: token
              expirationSeconds: 3600
```

</details>

---

## Final Tips

### Day Before Exam

- ✅ Review this guide
- ✅ Do killer.sh simulation again
- ✅ Practice kubectl speed commands
- ✅ Get good sleep

### During Exam

- ✅ Read questions carefully (which namespace? which node?)
- ✅ Verify your work before moving on
- ✅ Use kubectl explain and --help
- ✅ Flag hard questions, come back later
- ✅ Manage your time (5-7 min per question)
- ✅ Stay calm - you've got this!

### Most Important Topics

Based on exam weight and frequency:

1. **NetworkPolicies** - Understand additive nature
2. **SecurityContext** - Pod and container levels
3. **RBAC** - Roles vs ClusterRoles, Bindings
4. **Falco** - Writing custom rules
5. **Trivy** - Scanning images and SBOMs
6. **ServiceAccount tokens** - Custom mounting, expiration
7. **AppArmor** - Profile syntax, network rules
8. **Runtime security** - Detecting suspicious behavior
9. **Admission controllers** - ImagePolicyWebhook
10. **Supply chain** - SBOM generation and scanning

---

## Additional Resources

### Official Documentation

- CKS Curriculum: <https://github.com/cncf/curriculum>
- CKS Handbook: <https://docs.linuxfoundation.org/tc-docs/certification/lf-handbook2>
- Important Instructions: <https://docs.linuxfoundation.org/tc-docs/certification/important-instructions-cks>
- FAQ: <https://docs.linuxfoundation.org/tc-docs/certification/faq-cka-ckad-cks>

### Practice Labs

- Killercoda CKS: <https://killercoda.com/killer-shell-cks>
- Killercoda CKA: <https://killercoda.com/killer-shell-cka>
- Killer.sh CKS Simulator (2 sessions with exam registration)

### Community Resources

- Walid Shaari's CKS repo: <https://github.com/walidshaari/Certified-Kubernetes-Security-Specialist>
- Cloud Native Security Whitepaper
- CNCF Security landscape: <https://landscape.cncf.io/>

---

**Good luck with your CKS exam! 🚀**

_Last updated: December 2025_
