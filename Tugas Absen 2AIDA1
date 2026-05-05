require('dotenv').config();

const { MongoClient, ObjectId } = require('mongodb');
const rl = require('readline-sync');

const client = new MongoClient(process.env.MONGODB_URI);

async function main() {
  await client.connect();
  console.log('CONNECTED!!!!!');

  const conn = client.db('example').collection('products');

  while (true) {
    console.log('\n1. Tampilkan semua data');
    console.log('2. Input Data');
    console.log('3. Update Data');
    console.log('4. Delete Data');
    console.log('5. Keluar');

    const pilih = rl.question('Pilih menu: ');

    // READ
    if (pilih === '1') {
      console.log('\nList semua data:');

      const data = await conn.find().toArray();

      if (data.length === 0) {
        console.log('Data kosong.');
      } else {
        data.forEach((element, index) => {
          console.log(`${index + 1}. ${element._id} | ${element.name} | ${element.price}`);
        });
      }

    // CREATE
    } else if (pilih === '2') {
      console.log('\nInput data section');

      const name = rl.question('Input nama: ');
      const price = Number(rl.question('Input harga: '));

      await conn.insertOne({ name, price });

      console.log('Data berhasil ditambahkan!');

    // UPDATE
    } else if (pilih === '3') {
      console.log('\nUpdate data section');

      const id = rl.question('Masukkan ID data: ');
      const name = rl.question('Nama baru: ');
      const price = Number(rl.question('Harga baru: '));

      try {
        const result = await conn.updateOne(
          { _id: new ObjectId(id) },
          { $set: { name, price } }
        );

        if (result.matchedCount === 0) {
          console.log('Data tidak ditemukan!');
        } else {
          console.log('Data berhasil diupdate!');
        }
      } catch (err) {
        console.log('ID tidak valid!');
      }

    // DELETE
    } else if (pilih === '4') {
      console.log('\nDelete data section');

      const id = rl.question('Masukkan ID data yang ingin dihapus: ');

      try {
        const result = await conn.deleteOne({ _id: new ObjectId(id) });

        if (result.deletedCount === 0) {
          console.log('Data tidak ditemukan!');
        } else {
          console.log('Data berhasil dihapus!');
        }
      } catch (err) {
        console.log('ID tidak valid!');
      }

    // EXIT
    } else if (pilih === '5') {
      console.log('Bye!');
      await client.close();
      break;

    } else {
      console.log('Menu tidak valid!');
    }
  }
}

main().catch(console.error);
