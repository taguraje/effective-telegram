// tests/login.spec.ts
/// <reference types="node" />
import { test, expect } from '@playwright/test';
import 'dotenv/config';

test('login: valid credentials', async ({ page }) => {
  const targetUrl = process.env.GENESIS_TARGET_URL ?? 'https://genesis-inspiretest.datam.nz/sws/public/login.action';
  const username = process.env.GENESIS_USERNAME;
  const password = process.env.GENESIS_PASSWORD;
console.log(process.env.GENESIS_USERNAME);
  if (!username || !password) {
    throw new Error('Set GENESIS_USERNAME and GENESIS_PASSWORD before running this test.');
  }
  const response = await page.goto(targetUrl, { waitUntil: 'load' });

expect(response).not.toBeNull();

if (!response) {
  throw new Error('Navigation did not return a response');
}

expect(response.status()).toBe(200);

  await page.fill('#user', username);//Providing Username from an Env file
  await page.fill('input[type="password"]', password);//Providing password from an Env file
  await page.locator('#LoginButton').click();//Clicking on login button
  await page.locator('a[href="/vretrieve/start.action"]').first().click();//Logging in and clicking on the retrieve button
  await page.locator('#input1568').fill('920664664');//filling the customer number
  await page.locator('#input1577').selectOption({ label: 'Mail' });//Selecting the Document type
 //await page.locator('#input1575').selectOption({ label: 'RINV' });
  //await page.locator('input[name="#input1568"]').('920650450');
  await page.click('#searchButton');//Clicking on the search button
});
