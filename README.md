// delay.js
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

// imageUpload.js
const fetch = require('node-fetch');

async function uploadImage(image) {
  const formData = new FormData();
  formData.append('image', image);

  const response = await fetch('https://ondchackathon-production.up.railway.app/process_image/', {
    method: 'POST',
    body: formData,
  });

  if (!response.ok) {
    throw new Error('Failed to upload image');
  }

  return response.json();
}

module.exports = uploadImage;


// imageUpload.test.js
const uploadImage = require('./imageUpload');
const delay = require('./delay');

// Example image paths or Buffers to upload
const imagesToUpload = [
  // Add your images here. For testing, these could be Buffers or paths to image files.
];

describe('Sequential Image Uploads with Delay', () => {
  test('uploads images sequentially with a 20-second delay between each', async () => {
    for (const image of imagesToUpload) {
      const result = await uploadImage(image);
      console.log('Upload result:', result);
      // Check the result or perform assertions here
      
      await delay(20000); // Wait for 20 seconds before proceeding to the next upload
    }
  }, (imagesToUpload.length * 20000) + 10000); // Set timeout longer than total expected test duration
});
