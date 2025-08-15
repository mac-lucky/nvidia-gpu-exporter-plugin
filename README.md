# NVIDIA GPU Exporter Unraid Plugin

This plugin provides easy installation and management of the NVIDIA GPU Exporter on Unraid systems with automated dependency management.

## 🔄 Automated Updates

This plugin repository includes automated dependency management via GitHub Actions:

### Weekly Automated Checks

- **Schedule**: Every Monday at 9:00 AM UTC
- **Purpose**: Check for new releases of [nvidia_gpu_exporter](https://github.com/utkuozdemir/nvidia_gpu_exporter)
- **Action**: Automatically creates a Pull Request when a new version is detected

### What Gets Updated Automatically

1. **Plugin Version**: Updated in `<!ENTITY version>` declaration
2. **MD5 Checksum**: Calculated and updated for the new binary
3. **CHANGES Section**: Updated with new version information
4. **Validation**: XML syntax checking and build testing

### Manual Trigger

You can manually trigger the update workflow:

1. Go to **Actions** → **Auto Update NVIDIA GPU Exporter Release**
2. Click **Run workflow**
3. Optionally enable "Force update even if already latest"

## 📋 Workflow Features

### Comprehensive Validation

- ✅ Downloads and verifies new release binaries
- ✅ Calculates MD5 checksums automatically
- ✅ Validates XML plugin syntax
- ✅ Tests build process (if Makefile exists)
- ✅ Creates detailed Pull Request with release information

### Error Handling

- 🚨 Creates GitHub issues if automation fails
- 📊 Provides detailed workflow summaries
- 🔄 Includes rollback protection via PR review process

### Security

- 🔒 Uses GitHub's built-in `GITHUB_TOKEN`
- 🎯 Only updates specific version/checksum fields
- 👀 All changes are reviewed via Pull Request

## Features

- **Auto-discovery** of nvidia-smi metric fields
- **Web interface** for configuration
- **Prometheus metrics** exposed on configurable port (default: 9835)
- **Remote execution** support for nvidia-smi command
- **Systemd-style service** management
- **Real-time monitoring** of GPU metrics

## Prerequisites

- Unraid 6.9.0 or later
- **NVIDIA Driver Plugin**: Install the Nvidia Driver plugin by navigating to the "Apps" tab in your Unraid web UI and search for "Nvidia Driver"
- `nvidia-smi` command available (provided by the NVIDIA Driver plugin)

## Installation

### Step 1: Install NVIDIA Driver Plugin (Required)

**Before installing this plugin**, you must first install the NVIDIA Driver plugin:

1. Navigate to the **"Apps"** tab in your Unraid web UI
2. Search for **"Nvidia Driver"**
3. Click **Install** on the NVIDIA Driver plugin
4. Follow the setup instructions to install your GPU drivers
5. Verify `nvidia-smi` command works by checking **Tools** → **System Log**

### Step 2: Install NVIDIA GPU Exporter

#### Method 1: Community Applications (Recommended)

1. Install the **Community Applications** plugin if not already installed
2. Go to **Apps** tab in Unraid web interface
3. Search for "NVIDIA GPU Exporter"
4. Click **Install**

#### Method 2: Manual Installation

1. Download the plugin file: `nvidia_gpu_exporter.plg`
2. Go to **Plugins** tab in Unraid web interface
3. Click **Install Plugin**
4. Upload or paste the URL to the plugin file
5. Click **Install**

## Configuration

After installation:

1. Go to **Settings** → **NVIDIA GPU Exporter**
2. Configure the following options:

   - **Service**: Enable/Disable the exporter
   - **Port**: Web interface port (default: 9835)
   - **nvidia-smi Path**: Path to nvidia-smi binary
   - **Query Fields**: Auto-detect or manually specify metrics
   - **Log File**: Location for log output

3. Click **Apply** to save settings

## Usage

### Viewing Metrics

Once enabled, you can view the metrics by visiting:

```
http://YOUR_UNRAID_IP:9835/metrics
```

### Prometheus Configuration

Add this to your Prometheus configuration:

```yaml
scrape_configs:
  - job_name: "nvidia-gpu-exporter"
    static_configs:
      - targets: ["YOUR_UNRAID_IP:9835"]
    scrape_interval: 15s
```

### Grafana Dashboard

Import the official **NVIDIA GPU Metrics** dashboard from Grafana.com:

- **Dashboard ID**: 14574
- **Name**: NVIDIA GPU Metrics
- **Direct Link**: [https://grafana.com/grafana/dashboards/14574-nvidia-gpu-metrics/](https://grafana.com/grafana/dashboards/14574-nvidia-gpu-metrics/)

This dashboard provides comprehensive visualization of all GPU metrics including temperature, memory usage, power consumption, and utilization rates.

## Available Metrics

The exporter provides comprehensive GPU metrics including:

- **Temperature**: GPU temperature readings
- **Utilization**: GPU and memory utilization percentages
- **Memory**: Used and total memory
- **Power**: Power consumption
- **Clock Speeds**: Graphics and memory clock frequencies
- **Fan Speed**: Fan RPM (if supported)
- **Process Information**: Running processes and their memory usage

## Query Fields

### Auto-detect (Recommended)

Automatically discovers all available metrics from your GPU.

### Basic Fields

```
name,temperature.gpu,utilization.gpu,memory.used,memory.total
```

### Extended Fields

```
name,temperature.gpu,utilization.gpu,utilization.memory,memory.used,memory.total,power.draw,clocks.current.graphics,clocks.current.memory
```

### Custom Fields

You can specify any fields supported by `nvidia-smi --help-query-gpu`

## Troubleshooting

### Service Won't Start

1. Check if NVIDIA drivers are installed: `nvidia-smi`
2. Verify nvidia-smi path in settings
3. Check log file for errors

### No Metrics Displayed

1. Ensure service is running (green status in settings)
2. Check firewall settings
3. Verify the port is accessible
4. Review log file for errors

### Permission Issues

The plugin runs with appropriate permissions, but if you encounter issues:

1. Check that nvidia-smi is executable
2. Verify NVIDIA device permissions

## Log Files

View logs in the plugin settings page or directly:

```bash
tail -f /var/log/nvidia_gpu_exporter.log
```

## Uninstallation

1. Go to **Plugins** tab
2. Find "NVIDIA GPU Exporter"
3. Click **Remove**

This will:

- Stop the service
- Remove all plugin files
- Clean up configuration

## Support

For support and issues:

- GitHub Repository: https://github.com/utkuozdemir/nvidia_gpu_exporter
- Unraid Forums: Search for "NVIDIA GPU Exporter"
- Create issues on GitHub for bugs and feature requests

## Version History

### 1.3.2

- Initial Unraid plugin release
- Web-based configuration interface
- Service management integration
- Log file monitoring
- Auto-discovery of nvidia-smi fields

## Credits

- **Original Project**: [utkuozdemir/nvidia_gpu_exporter](https://github.com/utkuozdemir/nvidia_gpu_exporter)
- **Unraid Plugin**: Community contribution
- **Based on**: [a0s/nvidia-smi-exporter](https://github.com/a0s/nvidia-smi-exporter)
