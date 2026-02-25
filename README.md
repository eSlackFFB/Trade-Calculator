<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>eSlack Trade Calculator | Dynasty & Redraft Fantasy Football</title>
  <meta name="description" content="eSlack Fantasy Football trade calculator with dynasty and redraft values. 187 players across 12 cross-position tiers.">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&family=JetBrains+Mono:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <script crossorigin src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
  <script crossorigin src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
  <script crossorigin src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.23.9/babel.min.js"></script>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { background: #0B1220; font-family: 'Inter', sans-serif; -webkit-font-smoothing: antialiased; }
    input::placeholder { color: rgba(255,255,255,0.3); }
    button { font-family: inherit; }
    ::-webkit-scrollbar { width: 5px; }
    ::-webkit-scrollbar-track { background: transparent; }
    ::-webkit-scrollbar-thumb { background: rgba(47,128,237,0.3); border-radius: 3px; }
    ::-webkit-scrollbar-thumb:hover { background: rgba(47,128,237,0.5); }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }
    .fade-in { animation: fadeIn 0.3s ease; }
  </style>
</head>
<body>
  <div id="root"></div>
  <script type="text/babel">
    const { useState, useMemo, useCallback } = React;
    const LOGO = "data:image/png;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/4gHYSUNDX1BST0ZJTEUAAQEAAAHIAAAAAAQwAABtbnRyUkdCIFhZWiAH4AABAAEAAAAAAABhY3NwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAA9tYAAQAAAADTLQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAlkZXNjAAAA8AAAACRyWFlaAAABFAAAABRnWFlaAAABKAAAABRiWFlaAAABPAAAABR3dHB0AAABUAAAABRyVFJDAAABZAAAAChnVFJDAAABZAAAAChiVFJDAAABZAAAAChjcHJ0AAABjAAAADxtbHVjAAAAAAAAAAEAAAAMZW5VUwAAAAgAAAAcAHMAUgBHAEJYWVogAAAAAAAAb6IAADj1AAADkFhZWiAAAAAAAABimQAAt4UAABjaWFlaIAAAAAAAACSgAAAPhAAAts9YWVogAAAAAAAA9tYAAQAAAADTLXBhcmEAAAAAAAQAAAACZmYAAPKnAAANWQAAE9AAAApbAAAAAAAAAABtbHVjAAAAAAAAAAEAAAAMZW5VUwAAACAAAAAcAEcAbwBvAGcAbABlACAASQBuAGMALgAgADIAMAAxADb/2wBDAAUDBAQEAwUEBAQFBQUGBwwIBwcHBw8LCwkMEQ8SEhEPERETFhwXExQaFRERGCEYGh0dHx8fExciJCIeJBweHx7/2wBDAQUFBQcGBw4ICA4eFBEUHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh4eHh7/wAARCAD6AfQDASIAAhEBAxEB/8QAHQABAAEFAQEBAAAAAAAAAAAAAAcEBQYICQMCAf/EAFYQAAEDAwEEBgUIBQYKCAcAAAEAAgMEBQYRBxIhMQgTQVFhcRQiMoGRFSNCUmKhscEJFjNysjd1kpSi0SQlNDVDVmR0leEYRGNzgrPC0hcnNkdV4vD/xAAbAQEAAgMBAQAAAAAAAAAAAAAABAYBAwUCB//EADkRAAIBAwIDBQcEAAQHAAAAAAABAgMEEQUhEjFBE1FhcYEGIpGhsdHwFDLB4RUWM/EjNEJDU4LS/9oADAMBAAIRAxEAPwDTJERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREAREQBERAEREARFesaxW/5HLuWi2T1DQdHS6bsbfNx4L3TpzqS4YLL8DXVrU6MHOpJJLq9kWVFN2L7Cwd2XI7tpw1MFGPuL3fkFJFg2c4VZ911PYqeaRv+kqfnnf2uH3LuW/s5d1VmeIrx5/BFVvPbTTqD4aeZvw5fF/xk1RoqGtrX7lHSVFS76sUZefuCyCh2e5xW6ejYrdn68taZzfxW3lK2GnYI6eKOFg5NjaGge4K7Y7fq2xXRlfROaXjg9jxq2Rva0/3qc/ZqMYtueX8Pucb/AD3OdRJUlGPV54vl7v1NQWbHdprxq3Drjp47o/NeFZso2jUjN+oxG5tH2WB34ErqRil9t+RWhlwog0fRliOm9E7tafyParnJHDI0tkijcO4tBVcnGlTk4yi014r7F0ozua1NVKdWDT3Xuv8A+zkLc8ayG2AuuFkuNK0c3S0z2ge/TRWldfLvjljutHJS1ltpyx7dN5rA1zfEEcitbtqeymxU9xfR3uw0VVFKCaerZEI3vb+83Qhw7Qt9rZU7tuNOWJdz6+v9ELUdXr6ZFVLinxQ6uPTzT+5ooim3P9hlRSxSV2I1ElWxoLnUU5HWj9x3J3kdD5qFZopIZXwzRujkY4tcxw0LSOYI7Co1zaVbaXDUR0NP1S21CHHbyz3rqvNHwiIo50AiIgCIiAIiIAiIgCIiAIiIAiIgCIiAIiIAiIgCIiAIiIAiIgCIiAIiIAiIgCIiAIiIAiIgCIiAIiIAiIgCIiAIiIArjYLLc77XCjtdJJUSnidPZYO9x5AeayHAMErsjkbVVJdSWwHjKR60ngwfnyHip1sVst1loG0NspWU8LeYHNx73HmSrFpPs9VvcVKnuw+b8vuVbWvaalYZ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";

    const PLAYERS = [  { name: "Ja\'Marr Chase", pos: "WR", team: "", age: 25, redraft: 9500, dynasty: 10000, tier: 1 },
  { name: "Puka Nacua", pos: "WR", team: "", age: 24, redraft: 9372, dynasty: 9866, tier: 1 },
  { name: "Jaxon Smith-Nijgba", pos: "WR", team: "", age: 23, redraft: 8273, dynasty: 9733, tier: 1 },
  { name: "Amon-Ra St. Brown", pos: "WR", team: "", age: 26, redraft: 9120, dynasty: 9600, tier: 1 },
  { name: "Bijan Robinson", pos: "RB", team: "", age: 23, redraft: 7600, dynasty: 9500, tier: 2 },
  { name: "Jahmyr Gibbs", pos: "RB", team: "", age: 23, redraft: 7480, dynasty: 9350, tier: 2 },
  { name: "Justin Jefferson", pos: "WR", team: "", age: 26, redraft: 8740, dynasty: 9200, tier: 2 },
  { name: "Drake London", pos: "WR", team: "", age: 24, redraft: 8645, dynasty: 9100, tier: 3 },
  { name: "CeeDee Lamb", pos: "WR", team: "", age: 26, redraft: 8576, dynasty: 9028, tier: 3 },
  { name: "Malik Nabers", pos: "WR", team: "", age: 22, redraft: 7613, dynasty: 8957, tier: 3 },
  { name: "Jonathan Taylor", pos: "RB", team: "", age: 27, redraft: 9329, dynasty: 8885, tier: 3 },
  { name: "De\'Von Achane", pos: "RB", team: "", age: 24, redraft: 8373, dynasty: 8814, tier: 3 },
  { name: "James Cook", pos: "RB", team: "", age: 26, redraft: 8304, dynasty: 8742, tier: 3 },
  { name: "Brock Bowers", pos: "TE", team: "", age: 23, redraft: 6936, dynasty: 8671, tier: 3 },
  { name: "Trey McBride", pos: "TE", team: "", age: 26, redraft: 8170, dynasty: 8600, tier: 3 },
  { name: "Omarion Hampton", pos: "RB", team: "", age: 22, redraft: 2975, dynasty: 8500, tier: 4 },
  { name: "Ashton Jeanty", pos: "RB", team: "", age: 22, redraft: 2940, dynasty: 8400, tier: 4 },
  { name: "TreVeyon Henderson", pos: "RB", team: "", age: 23, redraft: 2905, dynasty: 8300, tier: 4 },
  { name: "Nico Collins", pos: "WR", team: "", age: 26, redraft: 7790, dynasty: 8200, tier: 4 },
  { name: "Josh Allen", pos: "QB", team: "", age: 29, redraft: 8505, dynasty: 8100, tier: 5 },
  { name: "Drake Maye", pos: "QB", team: "", age: 23, redraft: 5962, dynasty: 7950, tier: 5 },
  { name: "Christian McCaffrey", pos: "RB", team: "", age: 29, redraft: 6630, dynasty: 7800, tier: 5 },
  { name: "Tee Higgins", pos: "WR", team: "", age: 27, redraft: 7315, dynasty: 7700, tier: 6 },
  { name: "Chris Olave", pos: "WR", team: "", age: 25, redraft: 7288, dynasty: 7672, tier: 6 },
  { name: "Garrett Wilson", pos: "WR", team: "", age: 25, redraft: 7261, dynasty: 7644, tier: 6 },
  { name: "Emeka Egbuka", pos: "WR", team: "", age: 23, redraft: 2665, dynasty: 7616, tier: 6 },
  { name: "Ladd McConkey", pos: "WR", team: "", age: 24, redraft: 7208, dynasty: 7588, tier: 6 },
  { name: "Lamar Jackson", pos: "QB", team: "", age: 29, redraft: 7938, dynasty: 7560, tier: 6 },
  { name: "Joe Burrow", pos: "QB", team: "", age: 29, redraft: 7908, dynasty: 7532, tier: 6 },
  { name: "Justin Herbert", pos: "QB", team: "", age: 27, redraft: 7128, dynasty: 7504, tier: 6 },
  { name: "Jalen Hurts", pos: "QB", team: "", age: 27, redraft: 7102, dynasty: 7476, tier: 6 },
  { name: "Patrick Mahomes", pos: "QB", team: "", age: 30, redraft: 7820, dynasty: 7448, tier: 6 },
  { name: "Kyren Williams", pos: "RB", team: "", age: 25, redraft: 7049, dynasty: 7420, tier: 6 },
  { name: "Bucky Irving", pos: "RB", team: "", age: 23, redraft: 5913, dynasty: 7392, tier: 6 },
  { name: "Chase Brown", pos: "RB", team: "", age: 25, redraft: 6995, dynasty: 7364, tier: 6 },
  { name: "Breece Hall", pos: "RB", team: "", age: 24, redraft: 6969, dynasty: 7336, tier: 6 },
  { name: "Kenneth Walker", pos: "RB", team: "", age: 25, redraft: 6942, dynasty: 7308, tier: 6 },
  { name: "Saquon Barkley", pos: "RB", team: "", age: 28, redraft: 8008, dynasty: 7280, tier: 6 },
  { name: "Quinshon Judkins", pos: "RB", team: "", age: 22, redraft: 2538, dynasty: 7252, tier: 6 },
  { name: "Zay Flowers", pos: "WR", team: "", age: 25, redraft: 6862, dynasty: 7224, tier: 6 },
  { name: "Marvin Harrison Jr", pos: "WR", team: "", age: 23, redraft: 6116, dynasty: 7196, tier: 6 },
  { name: "George Pickens", pos: "WR", team: "", age: 24, redraft: 6809, dynasty: 7168, tier: 6 },
  { name: "Tetairoa McMillan", pos: "WR", team: "", age: 22, redraft: 2499, dynasty: 7140, tier: 6 },
  { name: "Rashee Rice", pos: "WR", team: "", age: 25, redraft: 6756, dynasty: 7112, tier: 6 },
  { name: "Colston Loveland", pos: "TE", team: "", age: 21, redraft: 2479, dynasty: 7084, tier: 6 },
  { name: "Sam LaPorta", pos: "TE", team: "", age: 25, redraft: 6703, dynasty: 7056, tier: 6 },
  { name: "Harold Fannin", pos: "TE", team: "", age: 21, redraft: 2459, dynasty: 7028, tier: 6 },
  { name: "Tyler Warren", pos: "TE", team: "", age: 23, redraft: 2450, dynasty: 7000, tier: 6 },
  { name: "Derrick Henry", pos: "RB", team: "", age: 33, redraft: 7934, dynasty: 6900, tier: 7 },
  { name: "Rome Odunze", pos: "WR", team: "", age: 23, redraft: 5810, dynasty: 6836, tier: 7 },
  { name: "Luther Burden", pos: "WR", team: "", age: 22, redraft: 2370, dynasty: 6772, tier: 7 },
  { name: "Tucker Kraft", pos: "TE", team: "", age: 25, redraft: 6373, dynasty: 6709, tier: 7 },
  { name: "Kyle Pitts", pos: "TE", team: "", age: 25, redraft: 6312, dynasty: 6645, tier: 7 },
  { name: "A.J. Brown", pos: "WR", team: "", age: 28, redraft: 6910, dynasty: 6581, tier: 7 },
  { name: "Devonta Smith", pos: "WR", team: "", age: 27, redraft: 6192, dynasty: 6518, tier: 7 },
  { name: "Davante Adams", pos: "WR", team: "", age: 33, redraft: 6776, dynasty: 6454, tier: 7 },
  { name: "Jameson Williams", pos: "WR", team: "", age: 24, redraft: 6070, dynasty: 6390, tier: 7 },
  { name: "Jayden Daniels", pos: "QB", team: "", age: 25, redraft: 6010, dynasty: 6327, tier: 7 },
  { name: "Caleb Williams", pos: "QB", team: "", age: 24, redraft: 5949, dynasty: 6263, tier: 7 },
  { name: "Brock Purdy", pos: "QB", team: "", age: 26, redraft: 5890, dynasty: 6200, tier: 7 },
  { name: "Brian Thomas", pos: "WR", team: "", age: 23, redraft: 5185, dynasty: 6100, tier: 8 },
  { name: "Travis Hunter", pos: "WR", team: "", age: 22, redraft: 2114, dynasty: 6041, tier: 8 },
  { name: "Jaylen Waddle", pos: "WR", team: "", age: 27, redraft: 5683, dynasty: 5983, tier: 8 },
  { name: "DK Metcalf", pos: "WR", team: "", age: 28, redraft: 6221, dynasty: 5925, tier: 8 },
  { name: "Michael Wilson", pos: "WR", team: "", age: 25, redraft: 5572, dynasty: 5866, tier: 8 },
  { name: "George Kittle", pos: "TE", team: "", age: 32, redraft: 6679, dynasty: 5808, tier: 8 },
  { name: "Josh Jacobs", pos: "RB", team: "", age: 27, redraft: 6037, dynasty: 5750, tier: 8 },
  { name: "Cam Skattebo", pos: "RB", team: "", age: 23, redraft: 1991, dynasty: 5691, tier: 8 },
  { name: "Travis Etienne", pos: "RB", team: "", age: 26, redraft: 5351, dynasty: 5633, tier: 8 },
  { name: "Javonte Williams", pos: "RB", team: "", age: 25, redraft: 5296, dynasty: 5575, tier: 8 },
  { name: "Trevor Lawrence", pos: "QB", team: "", age: 26, redraft: 5240, dynasty: 5516, tier: 8 },
  { name: "Bo Nix", pos: "QB", team: "", age: 25, redraft: 5185, dynasty: 5458, tier: 8 },
  { name: "Jaxon Dart", pos: "QB", team: "", age: 22, redraft: 1889, dynasty: 5400, tier: 8 },
  { name: "Xavier Worthy", pos: "WR", team: "", age: 22, redraft: 4505, dynasty: 5300, tier: 9 },
  { name: "Alec Pierce", pos: "WR", team: "", age: 25, redraft: 5007, dynasty: 5271, tier: 9 },
  { name: "DJ Moore", pos: "WR", team: "", age: 28, redraft: 5505, dynasty: 5243, tier: 9 },
  { name: "Terry McLaurin", pos: "WR", team: "", age: 30, redraft: 5736, dynasty: 5215, tier: 9 },
  { name: "Christian Watson", pos: "WR", team: "", age: 26, redraft: 4927, dynasty: 5187, tier: 9 },
  { name: "Mike Evans", pos: "WR", team: "", age: 32, redraft: 5673, dynasty: 5158, tier: 9 },
  { name: "Quentin Johnston", pos: "WR", team: "", age: 24, redraft: 4873, dynasty: 5130, tier: 9 },
  { name: "RJ Harvey", pos: "RB", team: "", age: 24, redraft: 1785, dynasty: 5102, tier: 9 },
  { name: "Jaylen Warren", pos: "RB", team: "", age: 27, redraft: 5327, dynasty: 5074, tier: 9 },
  { name: "D\'Andre Swift", pos: "RB", team: "", age: 27, redraft: 5298, dynasty: 5046, tier: 9 },
  { name: "Kyle Monangai", pos: "RB", team: "", age: 23, redraft: 1755, dynasty: 5017, tier: 9 },
  { name: "Blake Corum", pos: "RB", team: "", age: 25, redraft: 4739, dynasty: 4989, tier: 9 },
  { name: "Bhayshul Tuten", pos: "RB", team: "", age: 22, redraft: 1736, dynasty: 4961, tier: 9 },
  { name: "DJ Moore", pos: "WR", team: "", age: 28, redraft: 5179, dynasty: 4933, tier: 9 },
  { name: "Jayden Higgins", pos: "WR", team: "", age: 23, redraft: 1716, dynasty: 4905, tier: 9 },
  { name: "Jordan Addison", pos: "WR", team: "", age: 23, redraft: 1706, dynasty: 4876, tier: 9 },
  { name: "Jauan Jennings", pos: "WR", team: "", age: 28, redraft: 5090, dynasty: 4848, tier: 9 },
  { name: "Wan\'Dale Robinson", pos: "WR", team: "", age: 25, redraft: 4579, dynasty: 4820, tier: 9 },
  { name: "Michael Pittman", pos: "WR", team: "", age: 28, redraft: 5031, dynasty: 4792, tier: 9 },
  { name: "Khalil Shakir", pos: "WR", team: "", age: 25, redraft: 4525, dynasty: 4764, tier: 9 },
  { name: "Jakobi Meyers", pos: "WR", team: "", age: 29, redraft: 4971, dynasty: 4735, tier: 9 },
  { name: "Josh Downs", pos: "WR", team: "", age: 24, redraft: 4471, dynasty: 4707, tier: 9 },
  { name: "Parker Washington", pos: "WR", team: "", age: 23, redraft: 3977, dynasty: 4679, tier: 9 },
  { name: "Trey Benson", pos: "RB", team: "", age: 23, redraft: 3720, dynasty: 4651, tier: 9 },
  { name: "Tyler Allgeier", pos: "RB", team: "", age: 25, redraft: 4391, dynasty: 4623, tier: 9 },
  { name: "Woody Marks", pos: "RB", team: "", age: 25, redraft: 1607, dynasty: 4594, tier: 9 },
  { name: "Rhamondre Stevenson", pos: "RB", team: "", age: 27, redraft: 4794, dynasty: 4566, tier: 9 },
  { name: "Oronde Gadsden", pos: "TE", team: "", age: 22, redraft: 1588, dynasty: 4538, tier: 9 },
  { name: "Dalton Kincaid", pos: "TE", team: "", age: 26, redraft: 4284, dynasty: 4510, tier: 9 },
  { name: "Jake Ferguson", pos: "TE", team: "", age: 27, redraft: 4257, dynasty: 4482, tier: 9 },
  { name: "Dallas Goedert", pos: "TE", team: "", age: 31, redraft: 5120, dynasty: 4453, tier: 9 },
  { name: "Tyler Shough", pos: "QB", team: "", age: 26, redraft: 1548, dynasty: 4425, tier: 9 },
  { name: "Jared Goff", pos: "QB", team: "", age: 31, redraft: 4616, dynasty: 4397, tier: 9 },
  { name: "Baker Mayfield", pos: "QB", team: "", age: 30, redraft: 4587, dynasty: 4369, tier: 9 },
  { name: "Jordan Love", pos: "QB", team: "", age: 27, redraft: 4123, dynasty: 4341, tier: 9 },
  { name: "C.J. Stroud", pos: "QB", team: "", age: 24, redraft: 4096, dynasty: 4312, tier: 9 },
  { name: "Dak Prescott", pos: "QB", team: "", age: 32, redraft: 5997, dynasty: 4284, tier: 9 },
  { name: "Matthew Stafford", pos: "QB", team: "", age: 37, redraft: 7660, dynasty: 4256, tier: 9 },
  { name: "Bryce Young", pos: "QB", team: "", age: 24, redraft: 4016, dynasty: 4228, tier: 9 },
  { name: "Cam Ward", pos: "QB", team: "", age: 23, redraft: 1470, dynasty: 4200, tier: 9 },
  { name: "Chris Godwin", pos: "WR", team: "", age: 29, redraft: 4305, dynasty: 4100, tier: 10 },
  { name: "Brandon Aiyuk", pos: "WR", team: "", age: 27, redraft: 3847, dynasty: 4050, tier: 10 },
  { name: "Jalen McMillan", pos: "WR", team: "", age: 24, redraft: 3800, dynasty: 4000, tier: 10 },
  { name: "Ricky Pearsall", pos: "WR", team: "", age: 25, redraft: 3752, dynasty: 3950, tier: 10 },
  { name: "Alvin Kamara", pos: "RB", team: "", age: 30, redraft: 4290, dynasty: 3900, tier: 10 },
  { name: "Brian Robinson", pos: "RB", team: "", age: 26, redraft: 3657, dynasty: 3850, tier: 10 },
  { name: "Devin Neal", pos: "RB", team: "", age: 22, redraft: 1330, dynasty: 3800, tier: 10 },
  { name: "Tyrone Tracy", pos: "RB", team: "", age: 26, redraft: 3562, dynasty: 3750, tier: 10 },
  { name: "Chuba Hubbard", pos: "RB", team: "", age: 26, redraft: 3515, dynasty: 3700, tier: 10 },
  { name: "Tyjae Spears", pos: "RB", team: "", age: 24, redraft: 3467, dynasty: 3650, tier: 10 },
  { name: "Tony Pollard", pos: "RB", team: "", age: 28, redraft: 3780, dynasty: 3600, tier: 10 },
  { name: "Rico Dowdle", pos: "RB", team: "", age: 27, redraft: 3727, dynasty: 3550, tier: 10 },
  { name: "Jacory Croskey-Merritt", pos: "RB", team: "", age: 24, redraft: 1225, dynasty: 3500, tier: 10 },
  { name: "Kenneth Gainwell", pos: "RB", team: "", age: 26, redraft: 3277, dynasty: 3450, tier: 10 },
  { name: "J.K. Dobbins", pos: "RB", team: "", age: 27, redraft: 3570, dynasty: 3400, tier: 10 },
  { name: "David Montgomery", pos: "RB", team: "", age: 28, redraft: 3517, dynasty: 3350, tier: 10 },
  { name: "Aaron Jones", pos: "RB", team: "", age: 31, redraft: 4620, dynasty: 3300, tier: 10 },
  { name: "Rachaad White", pos: "RB", team: "", age: 27, redraft: 3412, dynasty: 3250, tier: 10 },
  { name: "Zach Charbonnet", pos: "RB", team: "", age: 25, redraft: 3040, dynasty: 3200, tier: 10 },
  { name: "Braelon Allen", pos: "RB", team: "", age: 22, redraft: 2480, dynasty: 3100, tier: 11 },
  { name: "Dylan Sampson", pos: "RB", team: "", age: 21, redraft: 1072, dynasty: 3064, tier: 11 },
  { name: "Jordan Mason", pos: "RB", team: "", age: 26, redraft: 2876, dynasty: 3028, tier: 11 },
  { name: "Romeo Doubs", pos: "WR", team: "", age: 25, redraft: 2842, dynasty: 2992, tier: 11 },
  { name: "Jayden Reed", pos: "WR", team: "", age: 25, redraft: 2808, dynasty: 2956, tier: 11 },
  { name: "Matthew Golden", pos: "WR", team: "", age: 22, redraft: 1021, dynasty: 2920, tier: 11 },
  { name: "Jerry Jeudy", pos: "WR", team: "", age: 26, redraft: 2739, dynasty: 2884, tier: 11 },
  { name: "Stefon Diggs", pos: "WR", team: "", age: 32, redraft: 3702, dynasty: 2848, tier: 11 },
  { name: "Deebo Samuel", pos: "WR", team: "", age: 30, redraft: 3093, dynasty: 2812, tier: 11 },
  { name: "Kayshon Boutte", pos: "WR", team: "", age: 23, redraft: 971, dynasty: 2776, tier: 11 },
  { name: "Kirk Cousins", pos: "QB", team: "", age: 37, redraft: 4932, dynasty: 2740, tier: 11 },
  { name: "Michael Penix", pos: "QB", team: "", age: 25, redraft: 2568, dynasty: 2704, tier: 11 },
  { name: "Sam Darnold", pos: "QB", team: "", age: 28, redraft: 2534, dynasty: 2668, tier: 11 },
  { name: "Daniel Jones", pos: "QB", team: "", age: 26, redraft: 2500, dynasty: 2632, tier: 11 },
  { name: "Kyler Murray", pos: "QB", team: "", age: 28, redraft: 2466, dynasty: 2596, tier: 11 },
  { name: "J.J. McCarthy", pos: "QB", team: "", age: 23, redraft: 1920, dynasty: 2560, tier: 11 },
  { name: "Mark Andrews", pos: "TE", team: "", age: 30, redraft: 2776, dynasty: 2524, tier: 11 },
  { name: "Theo Johnson", pos: "TE", team: "", age: 24, redraft: 2363, dynasty: 2488, tier: 11 },
  { name: "Brenton Strange", pos: "TE", team: "", age: 25, redraft: 858, dynasty: 2452, tier: 11 },
  { name: "Juwan Johnson", pos: "TE", team: "", age: 29, redraft: 2295, dynasty: 2416, tier: 11 },
  { name: "Mason Taylor", pos: "TE", team: "", age: 22, redraft: 833, dynasty: 2380, tier: 11 },
  { name: "T.J. Hockenson", pos: "TE", team: "", age: 28, redraft: 2226, dynasty: 2344, tier: 11 },
  { name: "Dalton Schultz", pos: "TE", team: "", age: 29, redraft: 2192, dynasty: 2308, tier: 11 },
  { name: "Isaiah Likely", pos: "TE", team: "", age: 25, redraft: 2158, dynasty: 2272, tier: 11 },
  { name: "Terrance Ferguson", pos: "TE", team: "", age: 22, redraft: 782, dynasty: 2236, tier: 11 },
  { name: "Hunter Henry", pos: "TE", team: "", age: 31, redraft: 2530, dynasty: 2200, tier: 11 },
  { name: "Jalen Coker", pos: "WR", team: "", age: 24, redraft: 735, dynasty: 2100, tier: 12 },
  { name: "Rashid Shaheed", pos: "WR", team: "", age: 27, redraft: 1957, dynasty: 2060, tier: 12 },
  { name: "Adonai Mitchell", pos: "WR", team: "", age: 23, redraft: 1717, dynasty: 2021, tier: 12 },
  { name: "Pat Bryant", pos: "WR", team: "", age: 23, redraft: 693, dynasty: 1982, tier: 12 },
  { name: "Tre\' Harris", pos: "WR", team: "", age: 23, redraft: 679, dynasty: 1942, tier: 12 },
  { name: "Isaac TesLaa", pos: "WR", team: "", age: 23, redraft: 666, dynasty: 1903, tier: 12 },
  { name: "Chimere Dike", pos: "WR", team: "", age: 22, redraft: 652, dynasty: 1864, tier: 12 },
  { name: "Elic Ayomanor", pos: "WR", team: "", age: 22, redraft: 638, dynasty: 1825, tier: 12 },
  { name: "Troy Franklin", pos: "WR", team: "", age: 22, redraft: 624, dynasty: 1785, tier: 12 },
  { name: "AJ Barner", pos: "TE", team: "", age: 23, redraft: 611, dynasty: 1746, tier: 12 },
  { name: "Chigoziem Okonkwo", pos: "TE", team: "", age: 26, redraft: 1621, dynasty: 1707, tier: 12 },
  { name: "Elijah Arroyo", pos: "TE", team: "", age: 22, redraft: 583, dynasty: 1667, tier: 12 },
  { name: "David Njoku", pos: "TE", team: "", age: 29, redraft: 1546, dynasty: 1628, tier: 12 },
  { name: "Cade Otton", pos: "TE", team: "", age: 26, redraft: 1509, dynasty: 1589, tier: 12 },
  { name: "Colby Parkinson", pos: "TE", team: "", age: 27, redraft: 1472, dynasty: 1550, tier: 12 },
  { name: "Ben Sinnott", pos: "TE", team: "", age: 23, redraft: 528, dynasty: 1510, tier: 12 },
  { name: "Jake Tonges", pos: "TE", team: "", age: 26, redraft: 514, dynasty: 1471, tier: 12 },
  { name: "Travis Kelce", pos: "TE", team: "", age: 36, redraft: 4009, dynasty: 1432, tier: 12 },
  { name: "Isiah Pacheco", pos: "RB", team: "", age: 26, redraft: 1322, dynasty: 1392, tier: 12 },
  { name: "Kaleb Johnson", pos: "RB", team: "", age: 22, redraft: 473, dynasty: 1353, tier: 12 },
  { name: "Tua Tagovailoa", pos: "QB", team: "", age: 27, redraft: 1248, dynasty: 1314, tier: 12 },
  { name: "Geno Smith", pos: "QB", team: "", age: 35, redraft: 2295, dynasty: 1275, tier: 12 },
  { name: "Jacoby Brissett", pos: "QB", team: "", age: 33, redraft: 1729, dynasty: 1235, tier: 12 },
  { name: "Mac Jones", pos: "QB", team: "", age: 27, redraft: 1136, dynasty: 1196, tier: 12 },
  { name: "Malik Willis", pos: "QB", team: "", age: 26, redraft: 1099, dynasty: 1157, tier: 12 },
  { name: "Justin Fields", pos: "QB", team: "", age: 26, redraft: 1061, dynasty: 1117, tier: 12 },
  { name: "Quinn Ewers", pos: "QB", team: "", age: 22, redraft: 377, dynasty: 1078, tier: 12 },
  { name: "Riley Leonard", pos: "QB", team: "", age: 23, redraft: 363, dynasty: 1039, tier: 12 },
  { name: "Shedeur Sanders", pos: "QB", team: "", age: 23, redraft: 350, dynasty: 1000, tier: 12 },
  { name: "2026 1st (Early/Mid)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 8500, tier: 4 },
  { name: "2026 1st (Mid/Late)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 6800, tier: 7 },
  { name: "2026 2nd (Early/Mid)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 4500, tier: 9 },
  { name: "2026 2nd (Mid/Late)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 3200, tier: 10 },
  { name: "2026 3rd (Early/Mid)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 2000, tier: 12 },
  { name: "2026 3rd (Mid/Late)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 1400, tier: 12 },
  { name: "2027 1st (Early/Mid)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 7800, tier: 5 },
  { name: "2027 1st (Mid/Late)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 6200, tier: 7 },
  { name: "2027 2nd (Early/Mid)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 4000, tier: 9 },
  { name: "2027 2nd (Mid/Late)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 2800, tier: 11 },
  { name: "2027 3rd (Early/Mid)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 1800, tier: 12 },
  { name: "2027 3rd (Mid/Late)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 1200, tier: 12 },
  { name: "2028 1st (Early/Mid)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 7200, tier: 6 },
  { name: "2028 1st (Mid/Late)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 5600, tier: 8 },
  { name: "2028 2nd (Early/Mid)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 3600, tier: 10 },
  { name: "2028 2nd (Mid/Late)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 2400, tier: 11 },
  { name: "2028 3rd (Early/Mid)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 1600, tier: 12 },
  { name: "2028 3rd (Mid/Late)", pos: "PICK", team: "\u2014", age: 0, redraft: 0, dynasty: 1000, tier: 12 },];

    /* ── Brand Palette ── */
    const C = {
      navy: "#0B1220",
      blue: "#2F80ED",
      lime: "#A3FF12",
      white: "#FFFFFF",
      graphite: "#2A2F3A",
      blueDim: "rgba(47,128,237,0.15)",
      limeDim: "rgba(163,255,18,0.12)",
      border: "rgba(47,128,237,0.12)",
      textMuted: "rgba(255,255,255,0.5)",
      textDim: "rgba(255,255,255,0.3)",
      cardBg: "rgba(42,47,58,0.5)",
    };

    const POS_COLORS = {
      QB: { bg: "#E53E3E", text: "#fff" },
      RB: { bg: "#38A169", text: "#fff" },
      WR: { bg: "#2F80ED", text: "#fff" },
      TE: { bg: "#D69E2E", text: "#fff" },
      PICK: { bg: "#805AD5", text: "#fff" },
    };

    const TIER_LABELS = {
      1: "Untouchable", 2: "Elite", 3: "Premium", 4: "High-End",
      5: "Strong", 6: "Starter+", 7: "Solid Starter", 8: "Good Starter",
      9: "Flex / Depth+", 10: "Depth", 11: "Bench Stash", 12: "Deep Bench"
    };

    const mono = "'JetBrains Mono', monospace";

    function getVerdict(diff, totalA, totalB) {
      const avg = (totalA + totalB) / 2;
      if (avg === 0) return { text: "Add players to both sides", color: C.textMuted, emoji: "" };
      const pct = Math.abs(diff) / avg * 100;
      if (pct < 3) return { text: "Dead Even", color: "#48BB78", emoji: "\u2696\uFE0F" };
      if (pct < 8) return { text: "Close Enough", color: C.lime, emoji: "\uD83E\uDD1D" };
      if (pct < 15) return { text: "Slight Edge", color: C.blue, emoji: "\uD83D\uDCCA" };
      if (pct < 25) return { text: "Lopsided", color: "#ED8936", emoji: "\u26A0\uFE0F" };
      return { text: "Robbery", color: "#E53E3E", emoji: "\uD83D\uDEA8" };
    }

    function PosBadge({ pos }) {
      const c = POS_COLORS[pos] || { bg: "#6b7280", text: "#fff" };
      return React.createElement("span", { style: {
        display: "inline-block", padding: "2px 8px", borderRadius: 4,
        fontSize: 10, fontWeight: 700, letterSpacing: 1,
        backgroundColor: c.bg, color: c.text, fontFamily: mono
      }}, pos);
    }

    function TierBadge({ tier }) {
      const isTop = tier <= 3;
      return React.createElement("span", { style: {
        fontSize: 10, color: isTop ? C.lime : C.blue, fontWeight: 600,
        fontFamily: mono, opacity: Math.max(0.4, 1 - (tier-1)*0.06)
      }}, "T" + tier);
    }

    /* ═══ RANKINGS ═══ */
    function RankingsView({ mode }) {
      const [posFilter, setPosFilter] = useState("ALL");
      const [sortKey, setSortKey] = useState("value");
      const [sortDir, setSortDir] = useState("desc");
      const [searchQ, setSearchQ] = useState("");
      const [expandedTiers, setExpandedTiers] = useState(new Set([1,2,3,4,5,6,7,8,9,10,11,12]));
      const positions = mode === "dynasty" ? ["ALL","QB","RB","WR","TE","PICK"] : ["ALL","QB","RB","WR","TE"];

      const filtered = useMemo(() => {
        let list = [...PLAYERS];
        if (mode === "redraft") list = list.filter(p => p.pos !== "PICK");
        if (posFilter !== "ALL") list = list.filter(p => p.pos === posFilter);
        if (searchQ) { const q = searchQ.toLowerCase(); list = list.filter(p => p.name.toLowerCase().includes(q)); }
        const vk = mode === "dynasty" ? "dynasty" : "redraft";
        if (sortKey === "value") list.sort((a,b) => sortDir === "desc" ? b[vk]-a[vk] : a[vk]-b[vk]);
        else if (sortKey === "name") list.sort((a,b) => sortDir === "asc" ? a.name.localeCompare(b.name) : b.name.localeCompare(a.name));
        else if (sortKey === "age") list.sort((a,b) => sortDir === "asc" ? a.age-b.age : b.age-a.age);
        return list;
      }, [posFilter, sortKey, sortDir, searchQ, mode]);

      const grouped = useMemo(() => {
        const g = {}; filtered.forEach(p => { if (!g[p.tier]) g[p.tier] = []; g[p.tier].push(p); }); return g;
      }, [filtered]);

      const toggleTier = t => setExpandedTiers(prev => { const n = new Set(prev); n.has(t)?n.delete(t):n.add(t); return n; });
      const handleSort = key => { if (sortKey===key) setSortDir(d=>d==="desc"?"asc":"desc"); else { setSortKey(key); setSortDir(key==="name"?"asc":"desc"); }};

      const SortHeader = ({label,sKey,width,align}) =>
        React.createElement("div",{onClick:()=>handleSort(sKey),style:{
          width, textAlign:align||"left", cursor:"pointer", userSelect:"none",
          fontSize:11, fontWeight:700, color:sortKey===sKey?C.lime:C.textMuted,
          letterSpacing:1, textTransform:"uppercase", fontFamily:mono,
          display:"flex", alignItems:"center", gap:4,
          justifyContent:align==="center"?"center":align==="right"?"flex-end":"flex-start"
        }}, label, sortKey===sKey && React.createElement("span",{style:{fontSize:9}},sortDir==="desc"?"\u25BC":"\u25B2"));

      const tierNums = Object.keys(grouped).map(Number).sort((a,b)=>a-b);
      let rank = 0;

      return React.createElement("div",{style:{maxWidth:900,margin:"0 auto"},className:"fade-in"},
        React.createElement("div",{style:{display:"flex",gap:12,marginBottom:16,flexWrap:"wrap",alignItems:"center"}},
          React.createElement("input",{type:"text",value:searchQ,placeholder:"Search player...",
            onChange:e=>setSearchQ(e.target.value),
            style:{flex:"1 1 200px",padding:"10px 14px",borderRadius:8,border:"1px solid "+C.border,
              background:C.cardBg,color:C.white,fontSize:14,outline:"none",fontFamily:"inherit",minWidth:160}}),
          React.createElement("div",{style:{display:"flex",gap:4,flexWrap:"wrap"}},
            positions.map(pos=>React.createElement("button",{key:pos,onClick:()=>setPosFilter(pos),style:{
              padding:"6px 12px",borderRadius:5,border:"1px solid",cursor:"pointer",fontFamily:mono,fontSize:11,fontWeight:600,letterSpacing:0.5,
              borderColor:posFilter===pos?C.blue:C.border,
              background:posFilter===pos?C.blueDim:"transparent",
              color:posFilter===pos?C.blue:C.textMuted}},pos)))
        ),
        React.createElement("div",{style:{display:"flex",gap:8,marginBottom:12,alignItems:"center"}},
          React.createElement("button",{onClick:()=>setExpandedTiers(new Set([1,2,3,4,5,6,7,8,9,10,11,12])),
            style:{padding:"4px 12px",borderRadius:4,border:"1px solid "+C.border,background:"transparent",color:C.textMuted,fontSize:11,cursor:"pointer",fontFamily:mono}},"Expand All"),
          React.createElement("button",{onClick:()=>setExpandedTiers(new Set()),
            style:{padding:"4px 12px",borderRadius:4,border:"1px solid "+C.border,background:"transparent",color:C.textMuted,fontSize:11,cursor:"pointer",fontFamily:mono}},"Collapse All"),
          React.createElement("span",{style:{fontSize:12,color:C.textDim,marginLeft:8}},filtered.length+" players")
        ),
        React.createElement("div",{style:{display:"flex",alignItems:"center",gap:10,padding:"8px 12px",borderBottom:"1px solid "+C.border,marginBottom:4}},
          React.createElement("div",{style:{width:36,textAlign:"center",fontSize:11,fontWeight:700,color:C.textDim,fontFamily:mono}},"#"),
          React.createElement("div",{style:{width:44}}),
          React.createElement(SortHeader,{label:"Player",sKey:"name",width:"auto"}),
          React.createElement("div",{style:{flex:1}}),
          React.createElement(SortHeader,{label:"Age",sKey:"age",width:"50px",align:"center"}),
          React.createElement(SortHeader,{label:mode==="dynasty"?"Dynasty":"Redraft",sKey:"value",width:"80px",align:"right"}),
          mode==="dynasty"&&React.createElement("div",{style:{width:70,textAlign:"right",fontSize:11,color:C.textDim,fontFamily:mono}},"RDR")
        ),
        tierNums.map(tn=>{
          const ps=grouped[tn], exp=expandedTiers.has(tn), vk=mode==="dynasty"?"dynasty":"redraft";
          const hi=Math.max(...ps.map(p=>p[vk])), lo=Math.min(...ps.map(p=>p[vk]));
          return React.createElement("div",{key:tn,style:{marginBottom:2}},
            React.createElement("div",{onClick:()=>toggleTier(tn),style:{
              display:"flex",alignItems:"center",gap:12,padding:"10px 12px",
              background:tn<=3?"rgba(163,255,18,0.04)":"rgba(47,128,237,0.04)",borderRadius:6,cursor:"pointer",
              borderLeft:"3px solid "+(tn<=3?C.lime:C.blue),
              borderLeftWidth:3,opacity:1,marginBottom:exp?2:0}},
              React.createElement("span",{style:{fontSize:12,color:C.textMuted,width:16}},exp?"\u25BC":"\u25B6"),
              React.createElement("span",{style:{fontSize:14,fontWeight:800,color:tn<=3?C.lime:C.blue,fontFamily:mono,minWidth:55}},"Tier "+tn),
              React.createElement("span",{style:{fontSize:12,color:C.white,fontWeight:600,opacity:0.6}},TIER_LABELS[tn]||""),
              React.createElement("div",{style:{flex:1}}),
              React.createElement("span",{style:{fontSize:11,color:C.textDim,fontFamily:mono}},ps.length+" player"+(ps.length!==1?"s":"")),
              React.createElement("span",{style:{fontSize:12,color:C.textMuted,fontFamily:mono,marginLeft:8}},lo.toLocaleString()+"\u2013"+hi.toLocaleString())
            ),
            exp&&ps.map((p,i)=>{
              rank++;
              const val=p[vk], bw=(val/10000)*100;
              return React.createElement("div",{key:p.name+i,style:{
                display:"flex",alignItems:"center",gap:10,padding:"7px 12px",
                borderBottom:"1px solid rgba(47,128,237,0.05)",position:"relative",overflow:"hidden"}},
                React.createElement("div",{style:{position:"absolute",left:0,top:0,bottom:0,width:bw+"%",
                  background:tn<=3?"rgba(163,255,18,0.03)":"rgba(47,128,237,0.03)",
                  borderRight:"1px solid "+(tn<=3?"rgba(163,255,18,0.1)":"rgba(47,128,237,0.08)"),pointerEvents:"none"}}),
                React.createElement("div",{style:{width:36,textAlign:"center",fontSize:12,fontWeight:600,color:C.textDim,fontFamily:mono,position:"relative"}},rank),
                React.createElement("div",{style:{position:"relative"}},React.createElement(PosBadge,{pos:p.pos})),
                React.createElement("div",{style:{flex:1,minWidth:0,position:"relative"}},
                  React.createElement("span",{style:{fontSize:14,fontWeight:600,color:C.white}},p.name)),
                React.createElement("div",{style:{width:50,textAlign:"center",fontSize:12,color:C.textMuted,fontFamily:mono,position:"relative"}},p.pos==="PICK"?"\u2014":p.age),
                React.createElement("div",{style:{width:80,textAlign:"right",fontSize:15,fontWeight:700,color:C.white,fontFamily:mono,position:"relative"}},val.toLocaleString()),
                mode==="dynasty"&&React.createElement("div",{style:{width:70,textAlign:"right",fontSize:12,color:C.textDim,fontFamily:mono,position:"relative"}},p.redraft.toLocaleString())
              );
            })
          );
        })
      );
    }


    /* ═══ POWER RANKINGS (Sleeper Integration) ═══ */
    function PowerRankingsView({ mode }) {
      const [leagueId, setLeagueId] = useState("");
      const [loading, setLoading] = useState(false);
      const [error, setError] = useState("");
      const [leagueData, setLeagueData] = useState(null);

      const SLEEPER_API = "https://api.sleeper.app/v1";

      /* Build a name lookup from our PLAYERS array */
      const playerValueMap = useMemo(() => {
        const m = {};
        PLAYERS.forEach(p => {
          /* Normalize name for matching */
          const key = p.name.toLowerCase().replace(/[^a-z]/g, "");
          m[key] = p;
        });
        return m;
      }, []);

      const findPlayer = (name, pos) => {
        if (!name) return null;
        const key = name.toLowerCase().replace(/[^a-z]/g, "");
        let match = playerValueMap[key];
        if (match) return match;
        /* Fuzzy: try last name */
        const parts = name.split(" ");
        if (parts.length >= 2) {
          const lastName = parts.slice(1).join("").toLowerCase().replace(/[^a-z]/g, "");
          for (const [k, v] of Object.entries(playerValueMap)) {
            if (k.includes(lastName) && v.pos === pos) return v;
          }
        }
        return null;
      };

      const fetchLeague = async () => {
        const id = leagueId.trim();
        if (!id) { setError("Please enter a Sleeper League ID"); return; }
        setLoading(true); setError(""); setLeagueData(null);

        try {
          /* 1. League info */
          const leagueRes = await fetch(SLEEPER_API + "/league/" + id);
          if (!leagueRes.ok) throw new Error("League not found. Check the ID and try again.");
          const league = await leagueRes.json();

          /* 2. Rosters */
          const rostersRes = await fetch(SLEEPER_API + "/league/" + id + "/rosters");
          const rosters = await rostersRes.json();

          /* 3. Users */
          const usersRes = await fetch(SLEEPER_API + "/league/" + id + "/users");
          const users = await usersRes.json();
          const userMap = {};
          users.forEach(u => { userMap[u.user_id] = u.display_name || u.username || "Unknown"; });

          /* 4. Sleeper player DB (cached) */
          let sleeperPlayers = window.__sleeperPlayersCache;
          if (!sleeperPlayers) {
            const pRes = await fetch(SLEEPER_API + "/players/nfl");
            sleeperPlayers = await pRes.json();
            window.__sleeperPlayersCache = sleeperPlayers;
          }

          /* 5. Fetch traded picks to determine pick ownership */
          const picksRes = await fetch(SLEEPER_API + "/league/" + id + "/traded_picks");
          const tradedPicks = await picksRes.json();

          /* Also try to get draft picks for future seasons via league history */
          /* Build a roster_id -> owner_id map */
          const rosterOwnerMap = {};
          const ownerRosterMap = {};
          rosters.forEach(r => {
            rosterOwnerMap[r.roster_id] = r.owner_id;
            ownerRosterMap[r.owner_id] = r.roster_id;
          });

          /* Default: each roster owns their own picks for all future years.
             Then apply traded_picks to reassign ownership. */
          const currentSeason = parseInt(league.season) || 2025;
          const futureYears = [currentSeason + 1, currentSeason + 2, currentSeason + 3];
          const rounds = [1, 2, 3];
          const numRosters = rosters.length;

          /* pickOwnership[rosterId] = array of { year, round, originalOwnerRosterId } */
          const pickOwnership = {};
          rosters.forEach(r => {
            pickOwnership[r.roster_id] = [];
            /* Start with own picks for each future year */
            futureYears.forEach(yr => {
              rounds.forEach(rd => {
                pickOwnership[r.roster_id].push({ year: yr, round: rd, originalRosterId: r.roster_id });
              });
            });
          });

          /* Apply trades: remove from previous_owner, add to owner */
          if (Array.isArray(tradedPicks)) {
            tradedPicks.forEach(tp => {
              if (!futureYears.includes(tp.season)) return;
              /* Remove from previous owner */
              const prevId = tp.previous_owner_id || tp.roster_id;
              const newId = tp.owner_id;
              if (pickOwnership[prevId]) {
                const idx = pickOwnership[prevId].findIndex(p => p.year === tp.season && p.round === tp.round && p.originalRosterId === tp.roster_id);
                if (idx !== -1) pickOwnership[prevId].splice(idx, 1);
              }
              /* Add to new owner */
              if (pickOwnership[newId]) {
                pickOwnership[newId].push({ year: tp.season, round: tp.round, originalRosterId: tp.roster_id });
              }
            });
          }

          /* Pick value lookup: determine if original team is playoff-caliber (top half = Mid/Late) or not (bottom half = Early/Mid)
             Use current record to estimate pick position */
          const rostersByRecord = rosters.slice().sort((a, b) => {
            const wA = a.settings ? (a.settings.wins || 0) : 0;
            const wB = b.settings ? (b.settings.wins || 0) : 0;
            const pA = a.settings ? (a.settings.fpts || 0) : 0;
            const pB = b.settings ? (b.settings.fpts || 0) : 0;
            return (wB - wA) || (pB - pA);
          });
          const topHalf = new Set(rostersByRecord.slice(0, Math.ceil(numRosters / 2)).map(r => r.roster_id));

          /* Map pick to our PLAYERS value entries */
          const pickValueLookup = {};
          PLAYERS.filter(p => p.pos === "PICK").forEach(p => { pickValueLookup[p.name] = p; });

          function getPickValue(year, round, originalRosterId) {
            const isGoodTeam = topHalf.has(originalRosterId);
            const slot = isGoodTeam ? "Mid/Late" : "Early/Mid";
            const pickName = year + " " + (round === 1 ? "1st" : round === 2 ? "2nd" : "3rd") + " (" + slot + ")";
            return pickValueLookup[pickName] || null;
          }

          /* 6. Map rosters to values */
          const teams = rosters.map(roster => {
            const ownerName = userMap[roster.owner_id] || "Unknown Owner";
            const wins = roster.settings ? roster.settings.wins || 0 : 0;
            const losses = roster.settings ? roster.settings.losses || 0 : 0;
            const ties = roster.settings ? roster.settings.ties || 0 : 0;
            const fpts = roster.settings ? (roster.settings.fpts || 0) + (roster.settings.fpts_decimal || 0) / 100 : 0;

            const playerIds = roster.players || [];
            const starters = new Set(roster.starters || []);

            const rosterPlayers = [];
            let totalValue = 0;
            const posValues = { QB: 0, RB: 0, WR: 0, TE: 0 };
            const posCounts = { QB: 0, RB: 0, WR: 0, TE: 0 };

            playerIds.forEach(pid => {
              const sp = sleeperPlayers[pid];
              if (!sp) return;
              const name = (sp.first_name || "") + " " + (sp.last_name || "");
              const pos = sp.position || "??";
              const team = sp.team || "FA";
              const age = sp.age || 0;
              const isStarter = starters.has(pid);

              const matched = findPlayer(name.trim(), pos);
              const val = matched ? (mode === "dynasty" ? matched.dynasty : matched.redraft) : 0;
              const tier = matched ? matched.tier : 13;

              rosterPlayers.push({ name: name.trim(), pos, team, age, value: val, tier, isStarter, matched: !!matched, isPick: false });
              totalValue += val;
              if (posValues.hasOwnProperty(pos)) {
                posValues[pos] += val;
                posCounts[pos]++;
              }
            });

            /* Add draft picks */
            const teamPicks = pickOwnership[roster.roster_id] || [];
            const pickItems = [];
            let pickValue = 0;
            teamPicks.forEach(pk => {
              const matched = getPickValue(pk.year, pk.round, pk.originalRosterId);
              const val = matched ? matched.dynasty : 0;
              const isOwn = pk.originalRosterId === roster.roster_id;
              const originalOwnerName = userMap[rosterOwnerMap[pk.originalRosterId]] || "??";
              const rdLabel = pk.round === 1 ? "1st" : pk.round === 2 ? "2nd" : "3rd";
              const slotLabel = topHalf.has(pk.originalRosterId) ? "Mid/Late" : "Early/Mid";
              const displayName = pk.year + " " + rdLabel + (isOwn ? "" : " (via " + originalOwnerName + ")");
              const tier = matched ? matched.tier : 12;

              pickItems.push({
                name: displayName, pos: "PICK", team: slotLabel, age: 0,
                value: val, tier, isStarter: false, matched: !!matched, isPick: true,
                year: pk.year, round: pk.round
              });
              pickValue += val;
            });

            /* Sort picks: by year then round */
            pickItems.sort((a, b) => (a.year - b.year) || (a.round - b.round));

            /* Sort players by value desc */
            rosterPlayers.sort((a, b) => b.value - a.value);

            if (mode === "dynasty") totalValue += pickValue;

            return {
              ownerId: roster.owner_id,
              rosterId: roster.roster_id,
              ownerName,
              wins, losses, ties, fpts,
              totalValue,
              posValues,
              posCounts,
              players: rosterPlayers,
              picks: pickItems,
              pickValue,
              starterValue: rosterPlayers.filter(p => p.isStarter).reduce((s, p) => s + p.value, 0),
              benchValue: rosterPlayers.filter(p => !p.isStarter).reduce((s, p) => s + p.value, 0),
            };
          });

          /* Sort by total value */
          teams.sort((a, b) => b.totalValue - a.totalValue);

          setLeagueData({ name: league.name, season: league.season, teams, totalTeams: league.total_rosters });
        } catch (err) {
          setError(err.message || "Failed to fetch league data");
        } finally {
          setLoading(false);
        }
      };

      const [expandedTeam, setExpandedTeam] = useState(null);
      const maxTeamValue = leagueData ? Math.max(...leagueData.teams.map(t => t.totalValue)) : 1;

      const h = React.createElement;

      /* ── Input Section ── */
      if (!leagueData) {
        return h("div", { style: { maxWidth: 900, margin: "0 auto" }, className: "fade-in" },
          h("div", { style: { background: C.cardBg, borderRadius: 16, padding: "32px 28px", border: "1px solid " + C.border, textAlign: "center" } },
            h("div", { style: { fontSize: 48, marginBottom: 12 } }, "\uD83C\uDFC6"),
            h("h2", { style: { fontSize: 22, fontWeight: 800, color: C.white, marginBottom: 6 } }, "League Power Rankings"),
            h("p", { style: { fontSize: 14, color: C.textMuted, marginBottom: 24, lineHeight: 1.6 } },
              "Enter your Sleeper League ID to see how every team stacks up using eSlack\u2019s dynasty values. ",
              h("br"), h("span", { style: { fontSize: 12, color: C.textDim } }, "Find it in your Sleeper app URL: sleeper.com/leagues/", h("strong", { style: { color: C.blue } }, "YOUR_LEAGUE_ID"))),
            h("div", { style: { display: "flex", gap: 10, maxWidth: 500, margin: "0 auto", flexWrap: "wrap", justifyContent: "center" } },
              h("input", { type: "text", value: leagueId, placeholder: "Paste Sleeper League ID...",
                onChange: e => setLeagueId(e.target.value),
                onKeyDown: e => e.key === "Enter" && fetchLeague(),
                style: { flex: "1 1 250px", padding: "12px 16px", borderRadius: 8, border: "1px solid " + C.border,
                  background: "rgba(0,0,0,0.3)", color: C.white, fontSize: 15, outline: "none", fontFamily: "inherit", minWidth: 200 } }),
              h("button", { onClick: fetchLeague, disabled: loading, style: {
                padding: "12px 28px", borderRadius: 8, border: "none", cursor: loading ? "wait" : "pointer",
                background: "linear-gradient(135deg, " + C.blue + ", #5BA3F5)", color: C.white,
                fontSize: 14, fontWeight: 700, fontFamily: "inherit", opacity: loading ? 0.6 : 1,
                transition: "opacity 0.2s" } }, loading ? "Loading..." : "Analyze League")),
            error && h("p", { style: { color: "#FC8181", fontSize: 13, marginTop: 12 } }, error)
          )
        );
      }

      /* ── Results ── */
      return h("div", { style: { maxWidth: 900, margin: "0 auto" }, className: "fade-in" },
        /* League Header */
        h("div", { style: { display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: 20, flexWrap: "wrap", gap: 12 } },
          h("div", null,
            h("h2", { style: { fontSize: 20, fontWeight: 800, color: C.white, marginBottom: 2 } }, leagueData.name),
            h("p", { style: { fontSize: 13, color: C.textMuted } },
              leagueData.totalTeams + " Teams \u00B7 " + (mode === "dynasty" ? "Dynasty" : "Redraft") + " Values \u00B7 " + leagueData.season)
          ),
          h("button", { onClick: () => { setLeagueData(null); setLeagueId(""); }, style: {
            padding: "8px 16px", borderRadius: 6, border: "1px solid " + C.border,
            background: "transparent", color: C.textMuted, cursor: "pointer", fontSize: 12, fontWeight: 600, fontFamily: "inherit"
          } }, "\u2190 New League")
        ),
        /* Team Cards */
        leagueData.teams.map((team, idx) => {
          const rank = idx + 1;
          const isExpanded = expandedTeam === team.rosterId;
          const barW = (team.totalValue / maxTeamValue) * 100;
          const medal = rank === 1 ? "\uD83E\uDD47" : rank === 2 ? "\uD83E\uDD48" : rank === 3 ? "\uD83E\uDD49" : "";
          const rankColor = rank <= 3 ? C.lime : rank <= 6 ? C.blue : rank <= 9 ? C.textMuted : "#FC8181";

          return h("div", { key: team.rosterId, style: { marginBottom: 8 } },
            /* Team Row */
            h("div", { onClick: () => setExpandedTeam(isExpanded ? null : team.rosterId), style: {
              display: "flex", alignItems: "center", gap: 12, padding: "12px 16px",
              background: isExpanded ? "rgba(47,128,237,0.08)" : C.cardBg,
              borderRadius: isExpanded ? "10px 10px 0 0" : 10, cursor: "pointer",
              border: "1px solid " + (isExpanded ? C.blue : C.border),
              borderBottom: isExpanded ? "none" : "1px solid " + C.border,
              transition: "all 0.2s", position: "relative", overflow: "hidden"
            } },
              /* Value bar bg */
              h("div", { style: { position: "absolute", left: 0, top: 0, bottom: 0, width: barW + "%",
                background: rank <= 3 ? "rgba(163,255,18,0.04)" : "rgba(47,128,237,0.03)",
                pointerEvents: "none" } }),
              /* Rank */
              h("div", { style: { width: 32, textAlign: "center", fontSize: 16, fontWeight: 900,
                color: rankColor, fontFamily: mono, position: "relative" } }, medal || rank),
              /* Name + Record */
              h("div", { style: { flex: 1, minWidth: 0, position: "relative" } },
                h("div", { style: { fontSize: 15, fontWeight: 700, color: C.white, whiteSpace: "nowrap", overflow: "hidden", textOverflow: "ellipsis" } }, team.ownerName),
                h("div", { style: { fontSize: 11, color: C.textMuted, fontFamily: mono } },
                  team.wins + "-" + team.losses + (team.ties ? "-" + team.ties : "") + (team.fpts > 0 ? " \u00B7 " + team.fpts.toFixed(1) + " pts" : ""))
              ),
              /* Position Breakdown Mini */
              h("div", { style: { display: "flex", gap: 4, position: "relative" } },
                ["QB", "RB", "WR", "TE"].map(pos => {
                  const pv = team.posValues[pos];
                  const maxPosVal = Math.max(...leagueData.teams.map(t => t.posValues[pos]), 1);
                  const strength = pv / maxPosVal;
                  return h("div", { key: pos, style: {
                    width: 36, height: 20, borderRadius: 4, display: "flex", alignItems: "center", justifyContent: "center",
                    fontSize: 9, fontWeight: 700, fontFamily: mono, letterSpacing: 0.5,
                    background: POS_COLORS[pos].bg + (strength > 0.85 ? "" : "80"),
                    color: C.white, opacity: Math.max(0.3, strength)
                  } }, pos);
                })
              ),
              /* Total */
              h("div", { style: { width: 80, textAlign: "right", position: "relative" } },
                h("div", { style: { fontSize: 17, fontWeight: 800, color: rank <= 3 ? C.lime : C.white, fontFamily: mono } },
                  (team.totalValue / 1000).toFixed(1) + "k"),
                h("div", { style: { fontSize: 10, color: C.textDim, fontFamily: mono } }, "value")
              ),
              h("span", { style: { fontSize: 12, color: C.textDim, position: "relative" } }, isExpanded ? "\u25B2" : "\u25BC")
            ),
            /* Expanded Roster */
            isExpanded && h("div", { style: {
              background: "rgba(47,128,237,0.04)", border: "1px solid " + C.blue,
              borderTop: "none", borderRadius: "0 0 10px 10px", padding: "12px 16px",
            }, className: "fade-in" },
              /* Starter/Bench value split */
              h("div", { style: { display: "flex", gap: 16, marginBottom: 12, flexWrap: "wrap" } },
                h("div", { style: { flex: 1, minWidth: 120, background: C.cardBg, borderRadius: 8, padding: "10px 14px", border: "1px solid " + C.border } },
                  h("div", { style: { fontSize: 11, color: C.textMuted, fontFamily: mono, marginBottom: 2 } }, "STARTERS"),
                  h("div", { style: { fontSize: 18, fontWeight: 800, color: C.lime, fontFamily: mono } }, team.starterValue.toLocaleString())
                ),
                h("div", { style: { flex: 1, minWidth: 120, background: C.cardBg, borderRadius: 8, padding: "10px 14px", border: "1px solid " + C.border } },
                  h("div", { style: { fontSize: 11, color: C.textMuted, fontFamily: mono, marginBottom: 2 } }, "BENCH"),
                  h("div", { style: { fontSize: 18, fontWeight: 800, color: C.blue, fontFamily: mono } }, team.benchValue.toLocaleString())
                ),
                mode === "dynasty" && h("div", { style: { flex: 1, minWidth: 120, background: C.cardBg, borderRadius: 8, padding: "10px 14px", border: "1px solid " + C.border } },
                  h("div", { style: { fontSize: 11, color: C.textMuted, fontFamily: mono, marginBottom: 2 } }, "PICKS"),
                  h("div", { style: { fontSize: 18, fontWeight: 800, color: "#805AD5", fontFamily: mono } }, team.pickValue.toLocaleString())
                ),
                ["QB", "RB", "WR", "TE"].map(pos => {
                  const posRank = leagueData.teams.slice().sort((a, b) => b.posValues[pos] - a.posValues[pos]).findIndex(t => t.rosterId === team.rosterId) + 1;
                  return h("div", { key: pos, style: { minWidth: 70, background: C.cardBg, borderRadius: 8, padding: "10px 14px", border: "1px solid " + C.border, textAlign: "center" } },
                    h("div", { style: { fontSize: 11, color: POS_COLORS[pos].bg, fontFamily: mono, fontWeight: 700, marginBottom: 2 } }, pos),
                    h("div", { style: { fontSize: 14, fontWeight: 800, color: C.white, fontFamily: mono } }, "#" + posRank)
                  );
                })
              ),
              /* Player List */
              h("div", { style: { fontSize: 11, color: C.textDim, fontFamily: mono, marginBottom: 6, letterSpacing: 1 } }, "ROSTER"),
              team.players.map((p, pi) =>
                h("div", { key: p.name + pi, style: {
                  display: "flex", alignItems: "center", gap: 8, padding: "5px 8px",
                  borderBottom: "1px solid rgba(47,128,237,0.05)",
                  opacity: p.value === 0 ? 0.4 : 1
                } },
                  p.isStarter && h("span", { style: { fontSize: 8, color: C.lime, fontWeight: 700, fontFamily: mono, width: 12 } }, "\u2605"),
                  !p.isStarter && h("span", { style: { width: 12 } }),
                  h(PosBadge, { pos: p.pos }),
                  h("span", { style: { flex: 1, fontSize: 13, color: C.white, fontWeight: p.isStarter ? 600 : 400 } }, p.name),
                  h("span", { style: { fontSize: 11, color: C.textDim, fontFamily: mono, width: 36 } }, p.team),
                  p.matched && h(TierBadge, { tier: p.tier }),
                  h("span", { style: { fontSize: 13, fontWeight: 600, color: p.value > 0 ? C.white : C.textDim, fontFamily: mono, width: 60, textAlign: "right" } },
                    p.value > 0 ? p.value.toLocaleString() : "\u2014")
                )
              ),
              /* Draft Picks Section */
              mode === "dynasty" && team.picks && team.picks.length > 0 && h("div", { style: { marginTop: 14, paddingTop: 12, borderTop: "1px solid rgba(128,90,213,0.15)" } },
                h("div", { style: { fontSize: 11, color: "#805AD5", fontFamily: mono, marginBottom: 6, letterSpacing: 1, fontWeight: 700 } },
                  "\uD83C\uDFB0 DRAFT CAPITAL (" + team.picks.length + " picks \u00B7 " + team.pickValue.toLocaleString() + " value)"),
                team.picks.map((pk, pi) =>
                  h("div", { key: "pk" + pk.name + pi, style: {
                    display: "flex", alignItems: "center", gap: 8, padding: "5px 8px",
                    borderBottom: "1px solid rgba(128,90,213,0.06)"
                  } },
                    h("span", { style: { width: 12 } }),
                    h(PosBadge, { pos: "PICK" }),
                    h("span", { style: { flex: 1, fontSize: 13, color: C.white, fontWeight: 500 } }, pk.name),
                    h("span", { style: { fontSize: 10, color: "#805AD5", fontFamily: mono, fontWeight: 600, padding: "1px 6px",
                      background: "rgba(128,90,213,0.15)", borderRadius: 3 } }, pk.team),
                    pk.matched && h(TierBadge, { tier: pk.tier }),
                    h("span", { style: { fontSize: 13, fontWeight: 600, color: pk.value > 0 ? C.white : C.textDim, fontFamily: mono, width: 60, textAlign: "right" } },
                      pk.value > 0 ? pk.value.toLocaleString() : "\u2014")
                  )
                )
              ),
              mode === "dynasty" && (!team.picks || team.picks.length === 0) && h("div", { style: { marginTop: 14, paddingTop: 12, borderTop: "1px solid rgba(128,90,213,0.15)" } },
                h("div", { style: { fontSize: 11, color: "#805AD5", fontFamily: mono, letterSpacing: 1 } }, "\uD83C\uDFB0 DRAFT CAPITAL"),
                h("div", { style: { fontSize: 12, color: C.textDim, fontStyle: "italic", padding: "8px" } }, "No future picks \u2014 all traded away")
              )
            )
          );
        })
      );
    }

    /* ═══ TRADE CALCULATOR ═══ */
    function PlayerCard({player,mode,onRemove}) {
      const val = mode==="dynasty"?player.dynasty:player.redraft;
      const showAge = mode==="dynasty"&&player.pos!=="PICK";
      return React.createElement("div",{style:{
        display:"flex",alignItems:"center",gap:10,padding:"8px 12px",
        background:C.cardBg,borderRadius:8,border:"1px solid "+C.border,marginBottom:6}},
        React.createElement(PosBadge,{pos:player.pos}),
        React.createElement("div",{style:{flex:1,minWidth:0}},
          React.createElement("div",{style:{fontSize:14,fontWeight:600,color:C.white,whiteSpace:"nowrap",overflow:"hidden",textOverflow:"ellipsis"}},player.name),
          React.createElement("div",{style:{fontSize:11,color:C.textMuted,display:"flex",gap:8}},
            showAge&&React.createElement("span",null,"Age "+player.age),
            React.createElement(TierBadge,{tier:player.tier}))),
        React.createElement("div",{style:{fontSize:16,fontWeight:700,color:C.white,fontFamily:mono}},val.toLocaleString()),
        React.createElement("button",{onClick:onRemove,style:{background:"none",border:"none",color:"#E53E3E",cursor:"pointer",fontSize:18,padding:"0 4px",lineHeight:1,opacity:0.6},
          onMouseEnter:e=>e.target.style.opacity=1,onMouseLeave:e=>e.target.style.opacity=0.6},"\u00D7")
      );
    }

    function PlayerSearch({onAdd,mode,excludeIds}) {
      const [q,setQ]=useState(""); const [pf,setPf]=useState("ALL"); const [foc,setFoc]=useState(false);
      const filtered=useMemo(()=>{
        if(!q&&pf==="ALL")return[];
        return PLAYERS.filter(p=>{
          if(excludeIds.has(p.name))return false;
          if(mode==="redraft"&&p.pos==="PICK")return false;
          if(pf!=="ALL"&&p.pos!==pf)return false;
          if(q)return p.name.toLowerCase().includes(q.toLowerCase());
          return true;
        }).sort((a,b)=>(mode==="dynasty"?b.dynasty-a.dynasty:b.redraft-a.redraft)).slice(0,15);
      },[q,pf,mode,excludeIds]);
      const positions=mode==="dynasty"?["ALL","QB","RB","WR","TE","PICK"]:["ALL","QB","RB","WR","TE"];
      return React.createElement("div",{style:{position:"relative",marginBottom:8}},
        React.createElement("div",{style:{display:"flex",gap:4,marginBottom:6,flexWrap:"wrap"}},
          positions.map(pos=>React.createElement("button",{key:pos,onClick:()=>setPf(pos),style:{
            padding:"3px 10px",borderRadius:4,border:"1px solid",cursor:"pointer",fontFamily:mono,fontSize:11,fontWeight:600,letterSpacing:0.5,
            borderColor:pf===pos?C.blue:C.border,background:pf===pos?C.blueDim:"transparent",color:pf===pos?C.blue:C.textMuted}},pos))),
        React.createElement("input",{type:"text",value:q,placeholder:"Search player...",
          onChange:e=>setQ(e.target.value),onFocus:()=>setFoc(true),onBlur:()=>setTimeout(()=>setFoc(false),200),
          style:{width:"100%",padding:"10px 14px",borderRadius:8,border:"1px solid "+C.border,
            background:C.cardBg,color:C.white,fontSize:14,outline:"none",boxSizing:"border-box",fontFamily:"inherit"}}),
        (foc||q)&&filtered.length>0&&React.createElement("div",{style:{
          position:"absolute",top:"100%",left:0,right:0,zIndex:50,
          background:"#0e1a2e",border:"1px solid "+C.border,borderRadius:8,marginTop:4,maxHeight:300,overflowY:"auto",
          boxShadow:"0 16px 48px rgba(0,0,0,0.6)"},className:"fade-in"},
          filtered.map((p,i)=>{
            const val=mode==="dynasty"?p.dynasty:p.redraft;
            return React.createElement("div",{key:p.name+p.pos+i,
              onMouseDown:()=>{onAdd(p);setQ("");},
              style:{display:"flex",alignItems:"center",gap:10,padding:"8px 12px",cursor:"pointer",borderBottom:"1px solid rgba(47,128,237,0.06)"},
              onMouseEnter:e=>e.currentTarget.style.background="rgba(47,128,237,0.08)",
              onMouseLeave:e=>e.currentTarget.style.background="transparent"},
              React.createElement(PosBadge,{pos:p.pos}),
              React.createElement("span",{style:{flex:1,fontSize:13,color:C.white,fontWeight:500}},p.name),
              React.createElement(TierBadge,{tier:p.tier}),
              React.createElement("span",{style:{fontSize:13,fontWeight:600,color:C.textMuted,fontFamily:mono}},val.toLocaleString()));
          }))
      );
    }

    function TradeSide({label,players,onAdd,onRemove,mode,total,color,accentDim}) {
      const excl=useMemo(()=>new Set(players.map(p=>p.name)),[players]);
      return React.createElement("div",{style:{flex:1,minWidth:280}},
        React.createElement("div",{style:{display:"flex",justifyContent:"space-between",alignItems:"baseline",marginBottom:12,paddingBottom:8,borderBottom:"2px solid "+color}},
          React.createElement("h3",{style:{margin:0,fontSize:13,fontWeight:700,color:C.textMuted,letterSpacing:2,textTransform:"uppercase",fontFamily:mono}},label),
          React.createElement("span",{style:{fontSize:22,fontWeight:800,color,fontFamily:mono}},total.toLocaleString())),
        React.createElement(PlayerSearch,{onAdd,mode,excludeIds:excl}),
        React.createElement("div",{style:{minHeight:60}},
          players.length===0&&React.createElement("div",{style:{textAlign:"center",padding:20,color:C.textDim,fontSize:13,fontStyle:"italic"}},"Search and select players above"),
          players.map((p,i)=>React.createElement(PlayerCard,{key:p.name+i,player:p,mode,onRemove:()=>onRemove(p.name)})))
      );
    }

    function CalculatorView({mode}) {
      const [sA,setSA]=useState([]); const [sB,setSB]=useState([]);
      const tA=useMemo(()=>sA.reduce((s,p)=>s+(mode==="dynasty"?p.dynasty:p.redraft),0),[sA,mode]);
      const tB=useMemo(()=>sB.reduce((s,p)=>s+(mode==="dynasty"?p.dynasty:p.redraft),0),[sB,mode]);
      const diff=tA-tB, verdict=getVerdict(diff,tA,tB);
      const addA=useCallback(p=>setSA(prev=>[...prev,p]),[]); const addB=useCallback(p=>setSB(prev=>[...prev,p]),[]);
      const remA=useCallback(n=>setSA(prev=>prev.filter(p=>p.name!==n)),[]); const remB=useCallback(n=>setSB(prev=>prev.filter(p=>p.name!==n)),[]);
      const mx=Math.max(tA,tB,1);
      return React.createElement("div",{style:{maxWidth:900,margin:"0 auto"},className:"fade-in"},
        (sA.length>0||sB.length>0)&&React.createElement("div",{style:{
          background:C.cardBg,borderRadius:12,padding:"16px 20px",border:"1px solid "+C.border,marginBottom:20}},
          React.createElement("div",{style:{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:10}},
            React.createElement("span",{style:{fontSize:13,color:C.textMuted,fontWeight:600}},verdict.emoji+" "+verdict.text),
            diff!==0&&tA>0&&tB>0&&React.createElement("span",{style:{fontSize:13,fontWeight:700,color:verdict.color,fontFamily:mono}},
              (diff>0?"Team A":"Team B")+" +"+Math.abs(diff).toLocaleString())),
          React.createElement("div",{style:{display:"flex",gap:6,height:10}},
            React.createElement("div",{style:{width:(tA/mx*100)+"%",borderRadius:5,transition:"width 0.4s ease",
              background:"linear-gradient(90deg, "+C.blue+", #5BA3F5)",minWidth:tA>0?8:0}}),
            React.createElement("div",{style:{width:(tB/mx*100)+"%",borderRadius:5,transition:"width 0.4s ease",
              background:"linear-gradient(90deg, "+C.lime+", #C8FF66)",minWidth:tB>0?8:0}}))
        ),
        React.createElement("div",{style:{display:"flex",gap:20,flexWrap:"wrap"}},
          React.createElement(TradeSide,{label:"Team A",players:sA,onAdd:addA,onRemove:remA,mode,total:tA,color:C.blue}),
          React.createElement(TradeSide,{label:"Team B",players:sB,onAdd:addB,onRemove:remB,mode,total:tB,color:C.lime})),
        (sA.length>0||sB.length>0)&&React.createElement("div",{style:{textAlign:"center",marginTop:20}},
          React.createElement("button",{onClick:()=>{setSA([]);setSB([]);},style:{
            padding:"8px 24px",borderRadius:8,border:"1px solid rgba(229,62,62,0.3)",
            background:"rgba(229,62,62,0.1)",color:"#FC8181",cursor:"pointer",fontSize:13,fontWeight:600},
            onMouseEnter:e=>e.target.style.background="rgba(229,62,62,0.2)",
            onMouseLeave:e=>e.target.style.background="rgba(229,62,62,0.1)"},"Clear All"))
      );
    }

    /* ═══ MAIN APP ═══ */
    function App() {
      const [view,setView]=useState("calculator");
      const [mode,setMode]=useState("dynasty");
      return React.createElement("div",{style:{minHeight:"100vh",background:C.navy,color:C.white,fontFamily:"'Inter', sans-serif",padding:"24px 16px"}},
        /* Header */
        React.createElement("div",{style:{maxWidth:900,margin:"0 auto 24px",textAlign:"center"}},
          React.createElement("img",{src:LOGO,alt:"eSlack Fantasy Football",style:{height:56,marginBottom:12}}),
          React.createElement("h1",{style:{margin:"0 0 2px",fontSize:30,fontWeight:900,letterSpacing:-0.5,color:C.white}},
            "Trade ",React.createElement("span",{style:{color:C.lime}},"Calculator")),
          React.createElement("p",{style:{margin:"6px 0 18px",color:C.textMuted,fontSize:13}},
            "PPR \u00B7 1QB \u00B7 8-Start Lineup \u00B7 Smart Dynasty Age-Weighting"),
          /* Nav Tabs */
          React.createElement("div",{style:{display:"flex",justifyContent:"center",gap:4,marginBottom:16,
            background:C.graphite,borderRadius:10,padding:4,display:"inline-flex"}},
            [{key:"calculator",icon:"\uD83D\uDCB1",label:"Calculator"},{key:"rankings",icon:"\uD83D\uDCCA",label:"Rankings"},{key:"power",icon:"\uD83C\uDFC6",label:"Power Rankings"}].map(v=>
              React.createElement("button",{key:v.key,onClick:()=>setView(v.key),style:{
                padding:"8px 20px",borderRadius:7,border:"none",cursor:"pointer",fontSize:13,fontWeight:600,
                background:view===v.key?C.blue:"transparent",
                color:view===v.key?C.white:C.textMuted,
                transition:"all 0.2s",fontFamily:"inherit"}},v.icon+" "+v.label))),
          /* Mode Toggle */
          React.createElement("div",{style:{display:"inline-flex",borderRadius:10,overflow:"hidden",
            border:"1px solid "+C.border,background:C.graphite,marginTop:4}},
            ["dynasty","redraft"].map(m=>React.createElement("button",{key:m,onClick:()=>setMode(m),style:{
              padding:"10px 28px",border:"none",cursor:"pointer",fontSize:13,fontWeight:700,letterSpacing:1,textTransform:"uppercase",
              background:mode===m?"linear-gradient(135deg, "+C.blue+", #5BA3F5)":"transparent",
              color:mode===m?C.white:C.textMuted,
              transition:"all 0.25s",fontFamily:mono}},m)))
        ),
        /* Content */
        view==="calculator"?React.createElement(CalculatorView,{mode}):view==="rankings"?React.createElement(RankingsView,{mode}):React.createElement(PowerRankingsView,{mode}),
        /* Footer Info */
        React.createElement("div",{style:{maxWidth:900,margin:"32px auto 0",padding:"20px 24px",
          background:C.cardBg,borderRadius:12,border:"1px solid "+C.border}},
          React.createElement("h4",{style:{margin:"0 0 10px",fontSize:12,color:C.lime,fontWeight:700,letterSpacing:1.5,textTransform:"uppercase",fontFamily:mono}},"How Values Work"),
          React.createElement("div",{style:{fontSize:13,color:C.textMuted,lineHeight:1.8}},
            React.createElement("p",{style:{margin:"0 0 6px"}},
              React.createElement("strong",{style:{color:C.blue}},"Dynasty")," values are mapped from eSlack\u2019s 12-tier cross-position rankings. Tier 1 (Untouchable) = 9,600-10,000 scaling down to Tier 12 (Deep Bench) = 1,000-2,100."),
            React.createElement("p",{style:{margin:"0 0 6px"}},
              React.createElement("strong",{style:{color:C.lime}},"Redraft")," values adjust for age and production. Older elite producers get a redraft premium (Henry, Kelce, Evans), while young/unproven players get a redraft discount."),
            React.createElement("p",{style:{margin:0,fontSize:12,color:C.textDim}},"1-10,000 scale \u00B7 PPR \u00B7 1QB \u00B7 8-Start"))
        ),
        React.createElement("div",{style:{textAlign:"center",marginTop:24}},
          React.createElement("img",{src:LOGO,alt:"eSlack",style:{height:28,opacity:0.4,marginBottom:6}}),
          React.createElement("div",{style:{fontSize:11,color:C.textDim}},"Dynasty Tiers \u00B7 Jan 2026"))
      );
    }

    ReactDOM.createRoot(document.getElementById("root")).render(React.createElement(App));
  </script>
</body>
</html>
