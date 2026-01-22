# Humana AI Innovation Library

A professional use case library showcasing AI implementations for Humana.

## 🚀 Quick Deploy to Vercel

### Method 1: Drag & Drop (Easiest)
1. Go to [vercel.com](https://vercel.com)
2. Click "Add New..." → "Project"
3. Drag the entire `humana-ai-library` folder into Vercel
4. Click "Deploy"
5. Your site will be live in ~30 seconds!

### Method 2: GitHub (Recommended for Updates)
1. Create a new repository on GitHub
2. Upload these files to the repository
3. In Vercel, click "Import Project"
4. Connect your GitHub repository
5. Click "Deploy"

## 📝 Customizing Your Content

All use cases are defined in `index.html` starting at line ~590 in the `useCasesData` array.

### Adding a New Use Case:

```javascript
{
    id: 7, // Increment this number
    title: "Your Use Case Title",
    category: "Member Services", // Or create new category
    description: "Brief description of what this does",
    businessContext: "Why this was needed and what problem it solves",
    technicalApproach: "How it was built technically",
    impact: "High Impact", // or "Medium Impact", "Very High Impact"
    timeToValue: "3 months",
    roi: "250%",
    technologies: ["Einstein AI", "Service Cloud", "etc"],
    metrics: {
        metric1: "60% improvement",
        metric2: "95% accuracy",
        metric3: "$2M savings"
    }
}
```

## 🎥 Adding Videos (Next Steps)

Videos can be added by:
1. **YouTube/Vimeo Embed** - Upload to YouTube, get embed code
2. **Self-hosted** - Upload MP4 files to Vercel's static folder
3. **Video Platform** - Use Wistia, Vidyard, or similar

Contact your developer to add video embedding features.

## 🎨 Customization

- **Colors**: Edit CSS variables in the `<style>` section (lines 15-20)
- **Logo**: Replace "HUMANA" text with an image tag
- **Categories**: Add new categories in the `useCasesData` array
- **Fonts**: Change Google Fonts import (line 8)

## 📱 Features

- ✅ Responsive design (mobile-friendly)
- ✅ Category filtering
- ✅ Detailed modal views
- ✅ Professional healthcare design
- ✅ Animated interactions
- ✅ No backend required (static site)

## 🔗 Custom Domain

To add your own domain (e.g., humana-ai-library.com):
1. Buy domain from Namecheap, Google Domains, etc.
2. In Vercel project settings → "Domains"
3. Add your domain and follow DNS instructions

## 📊 Analytics

To track visitors:
1. Go to Vercel project → "Analytics"
2. Or add Google Analytics by inserting tracking code in `<head>`

## 🆘 Support

Need help? Contact your Salesforce account team or the developer who created this.
