# MSP Network Monitoring Tool

A comprehensive network monitoring and management platform designed for Managed Service Providers (MSPs) to monitor client networks, devices, and infrastructure in real-time.

## 🚀 Features

- **Real-time Network Monitoring**: Monitor network segments, devices, and performance metrics
- **Device Management**: Add, configure, and monitor various network devices
- **Alert Management**: Comprehensive alerting system with multiple severity levels
- **Reporting & Analytics**: Generate detailed reports and analytics with charts
- **User Management**: Role-based access control with admin, technician, and viewer roles
- **Multi-language Support**: Support for English, Spanish, French, German, and Japanese
- **Responsive Design**: Works seamlessly on desktop, tablet, and mobile devices
- **API Integration**: RESTful API for external integrations and monitoring agents
- **Real-time Dashboard**: Live system metrics with customizable refresh intervals

## 🛠️ Technology Stack

- **Frontend**: Next.js 14, React 18, TypeScript
- **UI Components**: shadcn/ui, Tailwind CSS
- **Charts**: Recharts for data visualization
- **Icons**: Lucide React
- **Internationalization**: react-i18next
- **Database**: PostgreSQL (via Neon)
- **Authentication**: NextAuth.js
- **Deployment**: Vercel
- **Monitoring Agent**: Python with psutil

## 📋 Prerequisites

Before installing the MSP Network Monitoring Tool, ensure you have:

- **Node.js** 18.0 or higher
- **npm** or **yarn** package manager
- **PostgreSQL** database (or Neon account)
- **Git** for version control
- **Python 3.8+** for monitoring agents

## 🔧 Local Installation Guide

### Step 1: Clone the Repository

\`\`\`bash
git clone https://github.com/shaileshrichwal07/msp-network-monitor.git
cd msp-network-monitor
\`\`\`

### Step 2: Install Dependencies

\`\`\`bash
npm install
# or
yarn install
\`\`\`

### Step 3: Environment Setup

Create a `.env.local` file in the root directory:

\`\`\`env
# Database Configuration
DATABASE_URL="postgresql://username:password@localhost:5432/msp_network_monitor"

# NextAuth Configuration
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="your-secret-key-here"

# Optional: Redis for Caching
REDIS_URL="redis://localhost:6379"

# Optional: SMTP Configuration
SMTP_HOST="your-smtp-host"
SMTP_PORT="587"
SMTP_USER="your-smtp-user"
SMTP_PASS="your-smtp-password"

# Optional: External API Keys
SNMP_COMMUNITY="public"
API_KEY="your-api-key"
\`\`\`

### Step 4: Database Setup

#### Using PostgreSQL locally:

\`\`\`bash
# Install PostgreSQL (Ubuntu/Debian)
sudo apt update
sudo apt install postgresql postgresql-contrib

# Create database and user
sudo -u postgres createuser --interactive --pwprompt msp_user
sudo -u postgres createdb -O msp_user msp_network_monitor

# Update your .env.local with the correct DATABASE_URL
\`\`\`

#### Using Neon (Recommended):

1. Sign up at [neon.tech](https://neon.tech)
2. Create a new project
3. Copy the connection string to your `.env.local`

### Step 5: Initialize Database

\`\`\`bash
# Run database migrations (if using Prisma)
npx prisma migrate dev

# Or run custom SQL scripts
npm run db:setup
\`\`\`

### Step 6: Start Development Server

\`\`\`bash
npm run dev
# or
yarn dev
\`\`\`

Open [http://localhost:3000](http://localhost:3000) in your browser.

### Step 7: Default Login

Use these default credentials to log in:
- **Email**: admin@example.com
- **Password**: admin123

**⚠️ Important**: Change the default password immediately after first login.

## 🚀 Production Deployment Guide

### Deploy to Vercel (Recommended)

1. **Fork the repository** to your GitHub account

2. **Connect to Vercel**:
   - Go to [vercel.com](https://vercel.com)
   - Import your GitHub repository
   - Configure environment variables

3. **Set Environment Variables** in Vercel dashboard:
   \`\`\`
   DATABASE_URL=your-production-database-url
   NEXTAUTH_URL=https://your-domain.vercel.app
   NEXTAUTH_SECRET=your-production-secret
   \`\`\`

4. **Deploy**: Vercel will automatically deploy your application

### Using Docker

1. **Build and run with Docker Compose**:
   \`\`\`bash
   docker-compose up -d
   \`\`\`

2. **Or build manually**:
   \`\`\`bash
   docker build -t msp-monitor .
   docker run -p 3000:3000 msp-monitor
   \`\`\`

### Manual Server Deployment

1. **Build the application**:
   \`\`\`bash
   npm run build
   \`\`\`

2. **Start production server**:
   \`\`\`bash
   npm start
   \`\`\`

3. **Configure reverse proxy** (nginx example):
   ```nginx
   server {
       listen 80;
       server_name your-domain.com;
       
       location / {
           proxy_pass http://localhost:3000;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'upgrade';
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
           proxy_cache_bypass $http_upgrade;
       }
   }
   \`\`\`

### Automated Installation Script

For Ubuntu/Debian systems, use the automated installation script:

\`\`\`bash
chmod +x scripts/install.sh
sudo ./scripts/install.sh
\`\`\`

This script will:
- Install Node.js and dependencies
- Setup PostgreSQL database
- Configure Nginx reverse proxy
- Setup SSL with Let's Encrypt
- Configure PM2 for process management
- Install and configure the monitoring agent

## ⚙️ Configuration Guide

### Adding Network Segments

1. Navigate to the **Networks** tab
2. Click **Add Network**
3. Fill in the network details:
   - Network Name
   - Subnet (e.g., 192.168.1.0/24)
   - Gateway IP
   - VLAN ID (optional)
   - DNS servers
   - DHCP range (optional)
   - Location
   - Security level

### Adding Devices

1. Navigate to the **Devices** tab
2. Click **Add Device**
3. Configure device settings:
   - Device name
   - Device type (server, workstation, router, etc.)
   - IP address
   - MAC address (optional)
   - Location
   - Enable monitoring

### Setting Up Alerts

1. Go to the **Alerts** tab
2. Configure alert rules:
   - Alert conditions (CPU > 80%, Memory > 90%, etc.)
   - Severity levels (Critical, Warning, Info)
   - Notification methods (Email, SMS, Webhook)
   - Escalation policies

### User Management

1. Navigate to **Settings** → **Users**
2. Add users with appropriate roles:
   - **Admin**: Full access to all features
   - **Technician**: Monitor and manage devices
   - **Viewer**: Read-only access to dashboards

## 🤖 Monitoring Agent Installation

### Windows Agent

1. Download the agent installer from your monitoring dashboard
2. Run as administrator:
   \`\`\`cmd
   msp-agent-installer.exe --server=https://your-monitor.com --key=your-api-key
   \`\`\`

### Linux Agent

1. Download and install:
   \`\`\`bash
   wget https://your-monitor.com/agent/linux/msp-agent.deb
   sudo dpkg -i msp-agent.deb
   \`\`\`

2. Configure:
   \`\`\`bash
   sudo msp-agent configure --server=https://your-monitor.com --key=your-api-key
   sudo systemctl enable msp-agent
   sudo systemctl start msp-agent
   \`\`\`

### Python Agent (Manual Installation)

1. Install Python dependencies:
   \`\`\`bash
   pip install -r agent/requirements.txt
   \`\`\`

2. Configure the agent:
   ```python
   # Edit agent/monitoring_agent.py
   API_BASE_URL = "https://your-monitor.com/api"
   API_KEY = "your-api-key"
   CLIENT_ID = "your-client-id"
   \`\`\`

3. Run the agent:
   \`\`\`bash
   python agent/monitoring_agent.py
   \`\`\`

## 📡 API Documentation

### Authentication

All API requests require authentication using API keys:

\`\`\`bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
     https://your-domain.com/api/devices
\`\`\`

### Key Endpoints

- `GET /api/devices` - List all devices
- `POST /api/devices` - Add new device
- `PUT /api/devices/:id` - Update device
- `DELETE /api/devices/:id` - Delete device
- `GET /api/networks` - List network segments
- `POST /api/networks` - Add network segment
- `GET /api/alerts` - Get alerts
- `POST /api/alerts` - Create alert rule
- `POST /api/report` - Submit monitoring data
- `GET /api/stats` - Get system statistics

### Example API Usage

\`\`\`javascript
// Submit device metrics
const metrics = {
  device_id: "server-01",
  hostname: "web-server-01",
  ip_address: "192.168.1.10",
  cpu_usage: 45.2,
  memory_usage: 67.8,
  disk_usage: 23.1,
  timestamp: new Date().toISOString()
};

fetch('/api/report', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'X-API-Key': 'your-api-key'
  },
  body: JSON.stringify(metrics)
});
\`\`\`

## 🔍 Monitoring Features

### Real-time Dashboard
- Live system metrics with customizable refresh intervals
- Device status monitoring with color-coded indicators
- Performance charts for CPU, memory, disk, and network
- Temperature and power consumption monitoring

### Alert System
- Configurable thresholds for all metrics
- Multiple notification channels (Email, SMS, Webhook, Slack)
- Alert escalation and acknowledgment
- Maintenance windows to suppress alerts

### Reporting
- Performance reports with historical data
- Availability reports with uptime statistics
- Custom dashboards with drag-and-drop widgets
- Scheduled reports via email
- Data export in multiple formats (PDF, CSV, JSON)

### Network Discovery
- Automatic device discovery via IP scanning
- SNMP-based device identification
- Network topology mapping
- VLAN and subnet management

## 🛠️ Troubleshooting

### Common Issues

1. **Database Connection Issues**:
   \`\`\`bash
   # Test database connection
   npm run db:test
   
   # Check if PostgreSQL is running
   sudo systemctl status postgresql
   
   # Reset database
   npm run db:reset
   \`\`\`

2. **Permission Errors**:
   \`\`\`bash
   # Fix file permissions
   chmod -R 755 .
   chown -R www-data:www-data .
   \`\`\`

3. **Port Already in Use**:
   \`\`\`bash
   # Find process using port 3000
   lsof -i :3000
   
   # Kill process
   kill -9 <PID>
   \`\`\`

4. **Memory Issues**:
   \`\`\`bash
   # Increase Node.js memory limit
   export NODE_OPTIONS="--max-old-space-size=4096"
   npm start
   \`\`\`

### Logs and Debugging

Enable debug logging:

\`\`\`env
DEBUG=true
LOG_LEVEL=debug
NODE_ENV=development
\`\`\`

View logs:

\`\`\`bash
# Development
npm run dev

# Production with PM2
pm2 logs msp-monitor

# Docker logs
docker-compose logs -f
\`\`\`

### Performance Optimization

1. **Database Optimization**:
   - Add indexes for frequently queried columns
   - Use connection pooling
   - Regular database maintenance

2. **Caching**:
   - Enable Redis for session storage
   - Cache API responses
   - Use CDN for static assets

3. **Monitoring Agent Optimization**:
   - Adjust polling intervals based on network capacity
   - Use compression for data transmission
   - Implement data aggregation

## 🔒 Security Considerations

- **Use HTTPS in production** with valid SSL certificates
- **Regularly update dependencies** to patch security vulnerabilities
- **Implement proper access controls** with role-based permissions
- **Use strong passwords and API keys** with regular rotation
- **Enable audit logging** for all administrative actions
- **Configure firewall rules** to restrict access to necessary ports
- **Regular security assessments** and penetration testing
- **Backup encryption** for sensitive data
- **Two-factor authentication** for admin accounts

## 📞 Support and Contributing

### Getting Help

- Check the [documentation](https://docs.your-domain.com)
- Search [existing issues](https://github.com/your-username/msp-network-monitor/issues)
- Create a [new issue](https://github.com/your-username/msp-network-monitor/issues/new)
- Join our [Discord community](https://discord.gg/your-invite)

### Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Add tests for new functionality
5. Commit your changes (`git commit -m 'Add amazing feature'`)
6. Push to the branch (`git push origin feature/amazing-feature`)
7. Open a Pull Request

### Development Setup

\`\`\`bash
# Install development dependencies
npm install --include=dev

# Run tests
npm test

# Run linting
npm run lint

# Run type checking
npm run type-check
\`\`\`

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🗺️ Roadmap

### Version 1.1.0
- [ ] Real-time WebSocket updates
- [ ] Mobile app companion
- [ ] Advanced AI anomaly detection
- [ ] Integration with popular ticketing systems

### Version 1.2.0
- [ ] Custom dashboard widgets
- [ ] Multi-tenant architecture
- [ ] Advanced reporting templates
- [ ] Automated remediation actions

### Version 2.0.0
- [ ] Kubernetes monitoring
- [ ] Cloud infrastructure monitoring
- [ ] Machine learning-based predictions
- [ ] Advanced network topology visualization

## 🙏 Acknowledgments

- [Next.js](https://nextjs.org/) for the amazing React framework
- [shadcn/ui](https://ui.shadcn.com/) for the beautiful UI components
- [Recharts](https://recharts.org/) for the charting library
- [Tailwind CSS](https://tailwindcss.com/) for the utility-first CSS framework
- [Lucide](https://lucide.dev/) for the icon library

---

**Made with ❤️ for MSPs worldwide**

For more information, visit our [website](https://your-domain.com) or contact us at [support@your-domain.com](mailto:support@your-domain.com).
