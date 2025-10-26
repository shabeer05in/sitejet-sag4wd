# Contributing to SAG4WD Website

Thank you for your interest in contributing to the SAG4WD website project!

## Development Workflow

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Make your changes
4. Test your changes locally
5. Commit with clear messages: `git commit -m "Add: feature description"`
6. Push to your fork: `git push origin feature/your-feature-name`
7. Create a Pull Request

## Code Standards

### HTML
- Use semantic HTML5 elements
- Maintain proper indentation (4 spaces)
- Include appropriate meta tags
- Ensure accessibility (alt text, ARIA labels)

### CSS
- Follow existing naming conventions
- Use CSS variables for colors and common values
- Keep selectors specific but not overly complex
- Maintain responsive design principles
- Comment complex styles

### JavaScript
- Use modern ES6+ syntax
- Keep functions small and focused
- Add comments for complex logic
- Handle errors appropriately
- Test in multiple browsers

## File Organization

```
/assets/images/     - All image files
/assets/fonts/      - Custom fonts
/css/               - Stylesheets
/js/                - JavaScript files
```

## Adding New Pages

1. Create HTML file in root directory
2. Follow existing page structure from `index.html`
3. Link from navigation in header
4. Add entry to `sitemap.xml`
5. Update last modified date in sitemap

## Testing Before Commit

1. **Visual Testing**
   - Test in multiple browsers (Chrome, Firefox, Safari)
   - Test on mobile devices
   - Verify responsive breakpoints

2. **Performance Testing**
   - Optimize images before adding
   - Check file sizes
   - Verify lazy loading works

3. **Validation**
   - Validate HTML structure
   - Check CSS for errors
   - Verify JavaScript syntax

## Deployment

- Changes to `main`/`master` branch automatically deploy
- Check GitHub Actions for deployment status
- Verify changes on live site after deployment

## Questions?

Open an issue or contact the maintainers.

## License

By contributing, you agree that your contributions will be licensed under the same terms as the project.
