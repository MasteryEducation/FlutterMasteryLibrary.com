---
title: SVG Test
draft: true
---

{{< svg >}}


    // Initialize SVG.js
    const draw = SVG().addTo('#canvas').size(375, 812); // iPhone X screen size

    // Background
    draw.rect(375, 812).fill('#F2F2F7');

    // Status Bar
    draw.rect(375, 44).fill('#F9F9F9');
    draw.text('9:41 AM')
        .font({ size: 14, family: 'Helvetica' })
        .fill('#000')
        .move(16, 15);
    // Simplified status icons
    draw.circle(10).fill('#000').move(340, 17);

    // Navigation Bar
    draw.rect(375, 44).fill('#FFFFFF').move(0, 44);
    draw.text('Add Note')
        .font({ size: 18, family: 'Helvetica', weight: 'bold' })
        .fill('#000')
        .center(375 / 2, 44 + 12);
    draw.text('Cancel')
        .font({ size: 16, family: 'Helvetica' })
        .fill('#007AFF')
        .move(16, 56);

    // Text Area
    draw.rect(343, 200)
        .fill('#FFFFFF')
        .stroke({ color: '#C7C7CC', width: 1 })
        .radius(5)
        .move(16, 104);
    draw.text('Enter your note here...')
        .font({ size: 16, family: 'Helvetica' })
        .fill('#C7C7CD')
        .move(24, 120);

    // Save Button
    draw.rect(343, 50)
        .fill('#007AFF')
        .radius(10)
        .move(16, 324);
    draw.text('Save')
        .font({ size: 18, family: 'Helvetica', weight: 'bold' })
        .fill('#FFFFFF')
        .center(375 / 2, 324 + 15);

    // Optional: iPhone Outline

    draw.rect(375, 812)
      .fill('none')
      .stroke({ color: '#000', width: 2 })
      .radius(50);


{{< /svg >}}

