/**
    * Решение преподавателя решение на стороне сервера для поддрежки возможности отправки личных сообщений
     * Отправка сообщения всем слушателям
     *
     * @param message сообщение
     */
    private void broadcastMessage(String message) {
        String[] parts = message.split(" ");
        if (parts.length > 1 && parts[1].charAt(0) == '@' &&
                clients.stream().anyMatch(client -> client.name.equals(parts[1].substring(1)))) {
            var cln = clients.stream().filter(client -> client.name.equals(parts[1].substring(1))).findFirst();
            if (cln.isPresent()) {
                parts[1] = null;
                String newMessage = Arrays.stream(parts)
                        .filter(s -> s != null && !s.isEmpty())
                        .collect(Collectors.joining(" "));
                try {
                    cln.get().bufferedWriter.write(newMessage);
                    cln.get().bufferedWriter.newLine();
                    cln.get().bufferedWriter.flush();
                } catch (IOException e) {
                    closeEverything(socket, bufferedReader, bufferedWriter);
                }
            }
        } else {
            for (ClientManager client : clients) {
                try {
                    // Если клиент не равен по наименованию клиенту-отправителю,
                    // отправим сообщение
                    if (!client.name.equals(name) && message != null) {
                        client.bufferedWriter.write(message);
                        client.bufferedWriter.newLine();
                        client.bufferedWriter.flush();
                    }
                } catch (IOException e) {
                    closeEverything(socket, bufferedReader, bufferedWriter);
                }
            }
        }
    }
