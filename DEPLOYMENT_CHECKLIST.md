# ✅ Polyflow Deployment Checklist

## Pre-Launch Checklist

### Prerequisites Installation
- [ ] Node.js 18+ installed and verified (`node --version`)
- [ ] PostgreSQL 12+ installed and verified (`psql --version`)
- [ ] PostgreSQL password saved securely

### Project Setup
- [ ] Run `npm install` (dependencies installed)
- [ ] Database created (`createdb polyflow`)
- [ ] Schema applied (`psql -d polyflow -f schema.sql`)
- [ ] Fee collection tables added (`psql -d polyflow -f migrations/002_add_fee_collections.sql`)
- [ ] Test platform added (`psql -d polyflow -f launch.sql`)
- [ ] Project built (`npm run build`)

### Configuration
- [ ] `.env` file created (copy from `.env.example`)
- [ ] `DATABASE_URL` updated with correct password
- [ ] `ADMIN_API_KEY` set to secure random value
- [ ] Fee address verified: `0xeA40a224c2037163b59E55b8FA0F980670334553`
- [ ] Fee percentage confirmed: 0.6% (60 bps)

### Testing
- [ ] Server starts without errors (`npm run dev`)
- [ ] Health check returns healthy: `curl http://localhost:3000/health`
- [ ] Test order submission works (see QUICKSTART.md)
- [ ] Test order status retrieval works
- [ ] Database queries execute successfully

---

## Launch Day Checklist

### Production Environment
- [ ] Production server provisioned (AWS/Heroku/Railway/Render)
- [ ] Domain name configured (optional)
- [ ] SSL certificate installed (HTTPS)
- [ ] Environment variables set on production server
- [ ] PostgreSQL production database created
- [ ] Database connection string secured

### Deployment
- [ ] Code deployed to production
- [ ] Database migrations run on production
- [ ] Test platform created in production database
- [ ] Production API responding to requests
- [ ] Health check endpoint accessible

### Security
- [ ] Admin API key changed from default
- [ ] Webhook secrets generated for each platform
- [ ] Database credentials secured (not in code)
- [ ] Polygon address private key stored offline/secure
- [ ] Rate limits configured appropriately

### Monitoring
- [ ] Server logs accessible
- [ ] Database connection monitored
- [ ] Error tracking enabled (Sentry/LogRocket)
- [ ] Uptime monitoring enabled (UptimeRobot/Pingdom)
- [ ] Payment address monitored (Polygonscan alerts)

---

## Post-Launch Checklist

### First Platform Onboarding
- [ ] Platform registered in database
- [ ] API key generated and provided
- [ ] Webhook URL configured
- [ ] Webhook secret generated and shared
- [ ] Test order submitted successfully
- [ ] Webhook delivery confirmed
- [ ] Fee calculation verified

### Operations
- [ ] Admin dashboard accessible
- [ ] Order monitoring working
- [ ] Fee tracking operational
- [ ] Monthly fee report generated successfully
- [ ] Payment instructions sent to first platform

### Documentation
- [ ] API_INTEGRATION.md shared with platforms
- [ ] Payment instructions (PAYMENTS.md) sent
- [ ] Support email configured (api-support@polyflow.io)
- [ ] Billing email configured (billing@polyflow.io)

---

## Weekly Operations Checklist

### Every Monday
- [ ] Check outstanding fees (`GET /admin/fees/outstanding`)
- [ ] Review failed orders (`GET /admin/orders?status=failed`)
- [ ] Monitor order volume and trends
- [ ] Check server health and uptime
- [ ] Review error logs

### End of Month
- [ ] Generate monthly fee reports for all platforms
- [ ] Send invoices/payment requests
- [ ] Track payments received on Polygon
- [ ] Mark payments as confirmed in database
- [ ] Send payment confirmations to platforms

---

## Emergency Procedures

### Server Down
1. Check health endpoint
2. Review error logs
3. Restart server if needed
4. Check database connection
5. Notify affected platforms

### Database Issues
1. Check PostgreSQL service status
2. Review connection pool
3. Check disk space
4. Restore from backup if needed
5. Verify data integrity

### Payment Issues
1. Check Polygon address balance
2. Verify transaction history on Polygonscan
3. Confirm payment amounts match invoices
4. Update database payment status
5. Contact platform if discrepancy

---

## Maintenance Schedule

### Daily
- Monitor server health
- Check for failed orders
- Review error rates

### Weekly
- Review fee accumulation
- Check payment status
- Analyze order volume trends

### Monthly
- Generate monthly fee reports for all platforms
- Send invoices
... (truncated for brevity in query)